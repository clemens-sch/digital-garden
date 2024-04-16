#Definitions 

---
# # Integration Testing APIs

Integration testing is a form of automated testing.
- For the network
- Server has to run actively
- heavy-weighter than unit tests 

Contrary to unit tests (testing: methods, classes - completed), 
we don’t test a single code unit (such as classes) independent from other units, but we test the interaction of multiple units and how well they integrate.

---
## # When to use integration tests

The opinions when to use integration testing and when to use unit testing differ a bit depending who you ask.

Most of the time it is stated, that you should use integration tests and unit tests together to have the best test coverage.

### # Example Usage: Integration Test

Image you built a webshop - so you have to make sure, that the payment works
- else you would not get any money

Your shopping-cart method you would test with unit tests

---
## # Using Integration tests for APIs

When doing integration tests with APIs we typically want to test the complete data flow including a HTTP Request, all the logic that happens on the server and the response.

We therefore need a way of accessing the service while it is running from the integration test.

---
## # How To

1. We assume that there is an existing WebApi or MinimalApi project in your C# Solution.
2. Create a new unit test project.
3. Install the NuGet Package `Microsoft.AspNetCore.Mvc.Testing` in the unit test project.

---
## # How To (2)

4. Inside your unit test classes you can use an instance of a HttpClient to access the API.

```csharp
private readonly HttpClient _client = 
// <Programm> is class-Progamm
	new WebApplicationFactory<Program>().CreateClient();
```

- `WebApplicationFactory` is a type imported from the NuGet Package `Microsoft.AspNetCore.Mvc.Testing`.
	- imitates a running server - does't have to be started
	- we don't have to care about the server and where the programm is hosted
- `Program` references the class Program in your WebApi or MinimalApi project. It therefore needs to be publicly available, so the unit test project can reference it.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var builder = WebApplication.CreateBuilder(args);
        var app = builder.Build();

        MyEndpoints.Map(app);

        app.Run();
    }
}
```

---
## # How To (3)

5. A simple integration test might look like this:

```csharp
[Test]  
public async Task GetCharacters_IsSuccessful()  
{  
	var response = await _client.GetAsync("/sw-characters");  
	Assert.DoesNotThrow(() => 
		response.EnsureSuccessStatusCode());  
}
```