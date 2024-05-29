#Definitions 

[Main-Source](https://deep-thought.norwin.at/tech-kb/web-development/GraphQL/)

---
## # What is GraphQL?

- Yet another communication standard
- Mainly for client ⇔ server
- GraphQL is typically served over HTTP
    - Contrary to REST we use a single endpoint
- GraphQL services typically respond using JSON (yet not required by the spec)

---
## # Data model

With GraphQL you think of data as a graph of nodes.

![[Pasted image 20240321075828.png]]

---
## # Query language

GraphQL has its own language to query data

```graphql
{
  rewards {
    title
  }
}
```

```graphql
{
  rewards(id: 1) {
    title,
    savings {
      amount
    }
  }
}
```

here we get the specific title with the connected amounts.

---
## # Why?

- GraphQL gives power to the client.
- The client can specify what they want exactly for a specific use-case.
    - The server needs to be implemented in a way that supports the queries.
- This leads to very targeted queries fetching only the information that is needed.

---
## # Add GraphQL to an ASP.NET Core Web API

Using HotChocolate[1](https://deep-thought.norwin.at/tech-kb/web-development/GraphQL/#fn:1)

NuGet:

```powershell
Install-Package HotChocolate.AspNetCore
Install-Package HotChocolate.Data
Install-Package GraphQL.Server.Transports.AspNetCore
```

---
## # Add a query

```csharp
public class Query  
{  
    public IQueryable<Reward> GetRewards =>  
		new List<Reward>().AsQueryable();    
}
```

The methods can return any data. We just return an empty list here to get started.

---
## # Register with dependency injection

```csharp
// Program.cs

// Type Query points to Query class (previous slide)
builder.Services.AddGraphQLServer().AddQueryType<Query>()
// ...
app.MapGraphQL(path: "/graphql");
```

---
## # Run and verify

Run application and go to route /graphql in the browser.

Create a new document and try out the following query.

```graphql
query MyQuery {
  getRewards {
    title
  }
}
```

Should yield result:

```json
{
  "data": {
    "getRewards": []
  }
}
```

---
## # Add connection to database

**Attributes**

- UseProjection: Only selecting specific fields ("projection")
- UseFiltering: Allows filtering to query 
- UseSorting: Allows sorting the results
- Service: Something gets resolved using DI

```csharp
public class Query  
{  
    [UseProjection]  
    [UseFiltering]  
    [UseSorting]  
    public IQueryable<Reward> GetRewards(
	    [Service] RewardDbContext context) =>  
	        context.Rewards;  
}
```

```csharp
// Program.cs
builder.Services.AddGraphQLServer().AddQueryType<Query>()
  .AddProjections().AddFiltering().AddSorting();
```

---
## # Verify again

```graphql
query GetRewards {
  rewards {
    title
  }
}
```

returns

```json
{
  "data": {
    "rewards": [
      {
        "title": "X-Jam"
      }
    ]
  }
}
```

---
## # Get element by ID

```csharp
public class Query  
{    
  [UseFirstOrDefault]  
  [UseProjection]  
  public IQueryable<Reward?> GetReward(
    [Service] RewardDbContext context, 
    int id) =>  
      context.Rewards.Where(r => r.Id == id); 
}
```

```graphql
query GetReward {
  reward(id: 1) {
    title
  }
}
```

```json
{
  "data": {
    "reward": {
      "title": "X-Jam"
    }
  }
}
```

---
## # Add GraphQL to a Blazor WASM Client

Using StrawberryShake[2](https://deep-thought.norwin.at/tech-kb/web-development/GraphQL/#fn:2)

```powershell
# in root folder of solution
dotnet new tool-manifest
dotnet tool install StrawberryShake.Tools

# in Blazor Client project
Install-Package StrawberryShake.Blazor
```

---
## # Generate client

```powershell
# server needs to run for this command
# run command in root folder of solution
# use whatever url your server is serving from
dotnet graphql init https://localhost:7232/graphql/ `
-n RewardClient ` # give your generated client a name
-p SavingsPlanner/Client # point to the client project
```

---
## # Customize namespace

```json
// .graphqlrc.json
{  
  "schema": "schema.graphql",  
  "documents": "**/*.graphql",  
  "extensions": {  
    "strawberryShake": {  
      "name": "RewardClient",  
      "namespace": "SavingsPlanner.Client.GraphQL",  
      "url": "https://localhost:7232/graphql/",  
      //...
    }  
  }  
}
```

---
## # Write query definition

```graphql
# GetRewards.graphql
query GetRewards {  
  rewards {  
    title,  
    description,  
    targetAmount,  
    id  
  }  
}
```

---
## # Build project

```powershell
dotnet build
```

---
## # Register client with dependency injection

```csharp
// Program.cs
builder.Services  
    .AddGraphQLClient()  
    .ConfigureHttpClient(client =>  
        client.BaseAddress = new Uri("https://localhost:7080/graphql"));
```

---
## # Query data

```csharp
@inject RewardClient RewardClient
// ...
await RewardClient.GetRewards.ExecuteAsync();
```

---
## # Mutations

GraphQL Mutations let you modify data.

```graphql
mutation MyMutation {
  addReward(reward: {
    title: "Apple",
    description: "fruit",
    targetAmount: 1337,
    savingStart: "2023-03-01T00:00:00.000+01:00",
    savingEnd: "2023-04-01T00:00:00.000+01:00",
  }) {
    title,
    id
  }
}
```

You send the data and have control of what you get back.

---
## # Server Implementation

```csharp
public class Mutation  
{  
    public async Task<RewardListItemDto> AddReward(
      [Service] IRepository<Reward> rewardRepository, 
      AddRewardDto reward)  
    {  
        return await rewardRepository
          .AddAsync<AddRewardDto, RewardListItemDto>(reward);  
    }  
}
```

It is recommended to create specific types for adding items (e.g. without ID property).

---
## # Registration

```csharp
//Program.cs
builder.Services  
    .AddGraphQLServer()
    // ... your queries
    .AddMutationType<Mutation>();
```

---
## # Update Client

```powershell
# run from Client directory
# server needs to run
# correct server address needs to be specified in .graphqlrc
dotnet graphql update
```

---
## # Add Definition for Mutation

```graphql
# AddReward.graphql
mutation AddReward($rewardInput: AddRewardDtoInput!) {  
    addReward(reward: $rewardInput) {
        id  
    }  
}
```

---
## # Build again

```powershell
dotnet build
```

---
## # Use client to modify data

```csharp
// NewReward.razor
@inject RewardClient RewardClient
// ...
await RewardClient.AddReward.ExecuteAsync(
  new AddRewardDtoInput()  
  {  
    Title = _title,  
    TargetAmount = _targetAmount,  
    SavingStart = _startDate.Value,  
    SavingEnd = _endDate.Value,  
    Description = _description  
  });
```

---
## # Subscriptions

GraphQL offers an easy way to subscribe to events.  
The Server can notify the client of changes via a websocket connection.

![[Pasted image 20240411082214.png]]

---
## # Server Implementation

```csharp
public class Subscription  
{  
  [Subscribe]  
  public RewardListItemDto RewardAdded(
    [EventMessage] RewardListItemDto reward) 
    => reward;  
}
```

---
## # Send subscription message in Mutation

```csharp
public async Task<RewardListItemDto> AddReward(  
    [Service] IRepository<Reward> rewardRepository,   
    [Service] ITopicEventSender sender,  
    AddRewardDto reward)    
{    
    var addedReward = await rewardRepository  
        .AddAsync<AddRewardDto, RewardListItemDto>(reward);  
    await sender.SendAsync(
      nameof(Subscription.RewardAdded), addedReward);  
    return addedReward;  
}
```

---
## # Registration

```csharp
// Program.cs
builder.Services  
  .AddGraphQLServer()  
  // ... queries and mutations
  .AddInMemorySubscriptions()
  .AddSubscriptionType<Subscription>();

// ...

app.UseRouting();
app.UseWebSockets(); // add this line below UseRouting
```

---
## # Test subscriptions

```graphql
subscription MySubscription {
  rewardAdded {
    id,
    title
  }
}
```

---
## # Client implementation

```powershell
# run from Client directory
# server needs to run
# correct server address needs to be specified in .graphqlrc
dotnet graphql update

# Add NuGet Package
Install-Package StrawberryShake.Transport.WebSockets
```

---
## # Add subscription definition

```graphql
# SubscribeRewardAdded.graphql
subscription SubscribeRewardAdded {  
    rewardAdded {  
        id,  
        title,  
        description,  
        targetAmount,  
    }  
}
```

Build project to regenerate client afterwards.

---
## # Configuration

```csharp
// Program.cs
var graphQlUri = new Uri(
  builder.HostEnvironment.BaseAddress + "graphql");  
var graphQlWebSocketUri = new UriBuilder(graphQlUri)  
{  
    Scheme = Uri.UriSchemeWss  
}.Uri;

builder.Services.AddRewardClient()  
  .ConfigureHttpClient(client => 
    client.BaseAddress = graphQlUri)  
  .ConfigureWebSocketClient(client => 
    client.Uri = graphQlWebSocketUri);
```

---
## # Use subscription in client

```csharp
// Rewards.razor
@implements IDisposable
// ...
private IDisposable? rewardAddedSubscription;  
  
protected override async Task OnInitializedAsync()  
{  
  await LoadDataAsync();  
  rewardAddedSubscription = RewardClient  
    .SubscribeRewardAdded  
    .Watch()  
      .Subscribe(message =>  
      {  
        var dto = new RewardListItemDto(
          message.Data.RewardAdded.Id,  
          message.Data.RewardAdded.Title,  
          message.Data.RewardAdded.TargetAmount,  
          message.Data.RewardAdded.Description);  
	    rewards.Add(dto);  
		StateHasChanged();  
	  });  
}

public void Dispose()  
{  
    bookAddedSubscription?.Dispose();  
}
```

---

1. [Get started with GraphQL in .NET Core](https://chillicream.com/docs/hotchocolate/v13/get-started-with-graphql-in-net-core) [↩︎](https://deep-thought.norwin.at/tech-kb/web-development/GraphQL/#fnref:1)
    
2. [Get started with Strawberry Shake and Blazor](https://chillicream.com/docs/strawberryshake/v13/get-started) [↩︎](https://deep-thought.norwin.at/tech-kb/web-development/GraphQL/#fnref:2)

---