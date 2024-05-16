#Definitions 

[Main-Source](https://deep-thought.norwin.at/tech-kb/web-development/gRPC/)

---
# # gRPC

![[Pasted image 20240516091318.png]]

---
## # What does it mean?

| g   | gRPC      |
| --- | --------- |
| R   | Remote    |
| P   | Procedure |
| C   | Calls     |

---
## # What is it used for?

- Client ⇔ Server Communication
- Server ⇔ Server Communication
- X ⇔ Y Communication

---
## # Transport channel (ツ)

gRPC is agnostic about the transport channel.

For client server communication in a web application we use gRPC over HTTP.

---
## # Why use it rather than [X] ?

- Efficient serialization → Small message size
- Interface definition can easily be shared (.protobuf files)
- Support for both server-side code generation and client generation for many languages
- Integrating systems in different languages

---
## # Programming Model

ClientServerDoSomething()responseDataClientServer

> A client application calls a method on a server application on a different machine as if it were a local object.

---
## # Multiple Different Clients

MethodCall()ResponseMethodCall()ResponseClient AC#Client BC++ServerC#

---
## # Message Format

By default gRPC uses [Protocol Buffers](https://protobuf.dev/)

```proto
// This is how you define services and message types.

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}

message HelloRequest {
  string firstName = 1;
  string lastName = 2;
}

message HelloReply {
  string greeting = 1;
}
```

---
## # Adding gRPC and Protobuf to a .NET Solution

You need three projects.

![Project Structure](https://deep-thought.norwin.at//tech-kb/web-development/assets/gRPC-in-dotnet-project-structure.excalidraw.svg)

---

- Install NuGet Packages in Shared project:

```fallback
Grpc.Net.Client
Google.Protobuf
Grpc.Tools
```

- Add folder `Grpc` in Shared project
- Add reward.proto file in that folder

---

```protobuf
// File: reward.proto
syntax = "proto3";
option csharp_namespace = "SavingsPlanner.Shared.Grpc";
package Rewards;

service Rewards {
  rpc GetRewards (RewardRequest) returns (RewardReply);
}

message RewardRequest {}

message RewardReply {
  repeated RewardItem rewards = 1;
}

message RewardItem {
  int32 id = 1;
  string title = 2;
  double targetAmount = 3;
  string description = 4;
}
```

---

Add code in Shared .csproj file

```xml
<ItemGroup>
  <Protobuf 
    Include="Grpc/reward.proto" 
    GrpcServices="Server,Client">
  </Protobuf>
</ItemGroup>
```

---

- Install NuGet Packages in Client project:

```fallback
Grpc.Net.Client.Web
```

---

```csharp
// Add in Program.cs of client project
builder.Services.AddSingleton(services =>
{
  var httpClient = new HttpClient(
    new GrpcWebHandler(
      GrpcWebMode.GrpcWeb, 
      new HttpClientHandler()));
  var baseUri = services
    .GetRequiredService<NavigationManager>()
    .BaseUri;
  var channel = GrpcChannel
    .ForAddress(
      baseUri, 
      new GrpcChannelOptions { HttpClient = httpClient });
  return new Rewards.RewardsClient(channel);   
});
```

If you have a seperate Client-Server architecture - add the following line:

```csharp
builder.Services.AddSingleton(services =>  
{  
    var httpClient = new HttpClient(  
        ...
    !--
    var baseUri = @"http://localhost:5125";  
    !--
    var channel = GrpcChannel  
        ...  
    return new School.SchoolClient(channel);     
});
```

---

```cs
// in a Blazor component on the client
@inject Rewards.RewardsClient RewardsClient

private async Task LoadDataAsync()
{
  var rewardsResponse = 
    await RewardsClient.GetRewardsAsync(new RewardsRequest());
  rewards = rewardsResponse.Rewards.ToArray();
}
```

---

- Install NuGet Packages in Server project:

```fallback
Grpc.AspNetCore.Web
Grpc.AspNetCore
```

- Implement a service by deriving from generated classes in Shared project (next slide)

---

```cs
public class RewardService : Rewards.RewardsBase
{
  private readonly IRepository<Reward> _repository;
  public override async Task<RewardReply> GetRewards(
    RewardRequest request, serverCallContext context)
  {
    var reply = new RewardReply();
    var items = await _repository.GetAllAsync<RewardItem>();
    reply.Rewards.AddRange(items);
    return reply;
  }
}
```

---

```cs
// in Program.cs of Server project
builder.Services.AddGrpc();
app.UseGrpcWeb();
app.MapGrpcService<RewardService>().EnableGrpcWeb();
```

---

```cs
// Automapper configuration 
// for transformation to generated type
CreateMap<Reward, RewardItem>()
  .ForMember(
    r => r.TargetAmount,
    opt => opt.MapFrom(src => (double)src.TargetAmount));
```

