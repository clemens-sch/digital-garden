#Software-Engineering 
[DI](https://www.youtube.com/watch?v=KMErAbXRQdg)
[DI-Lifetimes](https://www.youtube.com/watch?v=7myI-l3Mrec)

---
## # What?

**Dependency Injection (DI)** is a software pattern for achieving **Inversion of Control (IoC)**.

Meaning that control is delegated to an external framework/container.

---
## # IoC

`Inversion of Controll`

The direction of dependency within the application should be in the direction of abstraction, not implementation details. Most applications are written such that compile-time dependency flows in the direction of runtime execution, producing a direct dependency graph. That is, if class A calls a method of class B and class B calls a method of class C, then at compile time class A will depend on class B, and class B will depend on class C.

---

![DirectDependencyGraph.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/DirectDependencyGraph.png)

---
## # IoC (2)

Applying the dependency inversion principle allows A to call methods on an abstraction (Interface/Base-Class) that B implements, making it possible for A to call B at run time, but for B to depend on an interface controlled by A at compile time (thus, _inverting_ the typical compile-time dependency). At run time, the flow of program execution remains unchanged, but the introduction of interfaces means that different implementations of these interfaces can easily be plugged in.

---

We use interfaces, that other Classes get independent of others. 
The classes should only be dependent on the Interface.

![InvertedDependencyGraph.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/InvertedDependencyGraph.png)

---

**Dependency inversion** is a key part of building loosely coupled applications, since implementation details can be written to depend on and implement higher-level abstractions, rather than the other way around. 
The resulting applications are more testable, modular, and maintainable as a result. 
The practice of _dependency injection_ is made possible by following the dependency inversion principle.

---
## # Dependency Injection in Asp.Net Core

Asp.Net Core delivers by default DI within it.

A _dependency_ is an object that another object depends on. Examine the following `MyDependency`class with a `WriteMessage` method that other classes depend on:

```csharp
public class MyDependency
{
    public void WriteMessage(string message)
    {
        Console.WriteLine(
	        $"MyDependency.WriteMessage called: {message}");
    }
}
```

---

A class can create an instance of the `MyDependency` class to make use of its `WriteMessage` method. In the following example, the `MyDependency` class is a dependency of the `IndexModel` class:

```csharp
public class IndexModel : PageModel
{
    private readonly MyDependency _dependency = 
	    new MyDependency(); // this is bad

    public void OnGet()
    {
        _dependency.WriteMessage("IndexModel.OnGet");
    }
}
```

---

The class creates and directly depends on the `MyDependency` class. Code dependencies, such as in the previous example, are problematic and should be avoided for the following reasons:

- To replace `MyDependency` with a different implementation, the `IndexModel` class must be modified.
- If `MyDependency` has dependencies, they must also be configured by the `IndexModel` class. In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.
- This implementation is difficult to unit test.

---
## # DI applied

Dependency injection addresses these problems through:

- The use of an interface or base class to abstract the dependency implementation.
- Registration of the dependency in a service container. ASP.NET Core provides a built-in service container,  [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider). Services are typically registered in the app’s `Program.cs` file.
- _Injection_ of the service into the constructor of the class where it’s used. The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it’s no longer needed.

---

```csharp
public interface IMyDependency
{
    void WriteMessage(string message);
}
```

```csharp
public class MyDependency : IMyDependency
{
    public void WriteMessage(string message)
    {
        Console.WriteLine(
	        $"MyDependency.WriteMessage: {message}");
    }
}
```

---

The sample app registers the `IMyDependency` service with the concrete type `MyDependency`. The  [AddScoped](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addscoped) method registers the service with a scoped lifetime, the lifetime of a single request.

```csharp
using DependencyInjectionSample.Interfaces;
using DependencyInjectionSample.Services;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();

// Registration
builder.Services.AddScoped<IMyDependency, MyDependency>();

var app = builder.Build();
```

---

The `IMyDependency` service is requested and used to call the `WriteMessage`method. Every type that is registered in the ServiceProvider can request dependencies.

```csharp
public class Index2Model : PageModel
{
    private readonly IMyDependency _myDependency;

	// Constructor injection
    public Index2Model(IMyDependency myDependency)
    {
        _myDependency = myDependency;            
    }

    public void OnGet()
    {
        _myDependency.WriteMessage("Index2Model.OnGet");
    }
}
```

---

By using the DI pattern, the controller or Razor Page:

- Doesn’t use the concrete type `MyDependency`, only the `IMyDependency` interface it implements. That makes it easy to change the implementation without modifying the controller or Razor Page.
- Doesn’t create an instance of `MyDependency`, it’s created by the DI container.

---
## # Service lifetimes

Services can be registered with one of the following lifetimes:

- Transient
- Scoped
- Singleton

---
### # Transient lifetime

New instance for every request - independent of other requests
- when the request is over - instance gets disposed

Transient lifetime services are created each time they’re requested from the service container. This lifetime works best for lightweight, stateless services. Register transient services with  [AddTransient](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addtransient).

---
### # Scoped lifetime

One instance for requests in the same scope ("Anwendungsbereich"); requests share the same instance
- when 2 clients get responds - they get two different instances
- when the request is over - instance gets disposed

For web applications, a scoped lifetime indicates that services are created once per client request. Register scoped services with  [AddScoped](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addscoped).

When using Entity Framework Core, the  [AddDbContext](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext`types with a scoped lifetime by default.

---
### # Scoped lifetime - Warning

Do _**not**_ resolve a scoped service from a singleton and be careful not to do so indirectly, for example, through a transient service. It may cause the service to have incorrect state when processing subsequent requests. It’s fine to:

- Resolve a singleton service from a scoped or transient service.
- Resolve a scoped service from another scoped or transient service.

---
### # Singleton lifetime

One instance, that is shared for all requests in the app.
- does not get disposed

Singleton lifetime services are created either:

- The first time they’re requested - a refresh does not change them.
- By the developer, when providing an implementation instance directly to the container. This approach is rarely needed.

Example: Multiplayer-Game: when waiting for 2 or more players within one instance.

---

Every subsequent request of the service implementation from the dependency injection container uses the same instance. If the app requires singleton behavior, allow the service container to manage the service’s lifetime. Don’t implement the singleton design pattern and provide code to dispose of the singleton. Services should never be disposed by code that resolved the service from the container. If a type or factory is registered as a singleton, the container disposes the singleton automatically.

Register singleton services with  [AddSingleton](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton). Singleton services must be thread safe and are often used in stateless services.

---
## # Registration

```csharp
builder.Services
	.AddTransient<IMyDependency, MyDependency>();
builder.Services
	.AddScoped<IMyDependency, MyDependency>();
builder.Services
	.AddSingleton<IMyDependency, MyDependency>();
```

You can also register a type without an interface.

```csharp
builder.Services
	.AddTransient<MyDependency>();
builder.Services
	.AddScoped<MyDependency>();
builder.Services
	.AddSingleton<MyDependency>();
```

---
## # Factory based dependency creation

Assume you have the following class. The class needs a prefix and suffix for instantiation. These values should be provided on startup from a configuration file.

```csharp
public class LabelGenService
{
    public string Prefix { get; }
    public string Suffix { get; }
    public LabelGenService(string prefix, string suffix)
    {
        Prefix = prefix;
        Suffix = suffix;
    }
    public string Generate()
    {
        return $"{Prefix}{DateTime.Now}{Suffix}";
    }
}
```

---

Add the following to your appsettings.json

```json
{
  "LabelGenOptions": {
    "Prefix": "CD",
    "Suffix": "MZ"
  }
}
```

Create a class for these options:

```csharp
public class LabelGenOptions {
	public string Prefix { get; set; }
	public string Suffix { get; set; }
}
```

Register these options:

```csharp
services.AddOptions<LabelGenOptions>()
	.BindConfiguration("LabelGenOptions");
```

---

Add a factory method to your service provider to register the `LabelGenService` and define how it should be instantiated.

```csharp
builder.services
	.AddSingleton<LabelGenService>(serviceProvider =>
    {
        var options = serviceProvider
            .GetService<IOptions<LabelGenOptions>>()!.Value;
        return new LabelGenService(
            options.Prefix,
            options.Suffix);
    });
```

---
### # DI Example

bag = DI-Container
water bottle = implementation of services

Image a person wants to go hiking. For hiking he takes a bag with him. 
This bag is used because he wants to store a water bottle, money and other stuff in it.

So he is able to use his filled bag in order to drink water.

![[Pasted image 20240312182237.png]]

---
### # Final Comparison

#### # Without DI

every page creates a new object 
- not scalable

for changes, we would have to change every Instance of the object

![[Pasted image 20240312183000.png]]

![[Pasted image 20240312183217.png]]

#### # With DI

framework looks in DI-Container for the IEmail. Then it creates the object and passes it to the page.

for changes, we just have to modify the builder method for the new implementation.

![[Pasted image 20240312183311.png]]

![[Pasted image 20240312183526.png]]

---
### # DI-Lifetime Example

We have a Web-Application that generates a GUID (Globally Unique Identifier), when the method GetGuid() is called.

Here we have (for each 2 objects):
- 1 Transient Service
- 1 Scoped Service
- 1 Singleton Service

![[Pasted image 20240312190931.png]]

When reloading our application:
- `Transient` - the GUID for both changes, their GUID does also not match
- `Scoped` - the GUID for both changes, but their GUID matches
- `Singleton` - the GUID stays for both the same

---