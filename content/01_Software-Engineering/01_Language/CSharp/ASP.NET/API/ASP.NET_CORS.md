#Software-Engineering 
[Source](https://www.stackhawk.com/blog/net-cors-guide-what-it-is-and-how-to-enable-it/)

---
## # What is CORS ?

`Cross Origin Resource Sharing`

Meaning a security mechanism, which allows web applications to access resources hosted on different domains/origins. 

Example: We have an API running on our localhost. We want to access this API through our Blazor Application. 
When we want to access the APIs data we get the following response:

```error
Access to fetch at 'https://localhost:7246/WeatherForecast' from origin 'http://localhost:3000'  
has been blocked by CORS policy:  
No 'Access-Control-Allow-Origin' header is present on the requested resource.  
If an opaque response serves your needs, set the request's mode to 'no-cors'  
to fetch the resource with CORS disabled.
```

---
## # Enabling CORS

Open the `Program.cs` (Startup.cs) file.

Add the following lines to you file

```csharp
var corsOrigin = "corsOrigin";

// builder initialization
builder.Services.AddCors(options =>  
{  
    options.AddPolicy(corsOrigin,  
        policy  =>  
        {  
                policy.AllowAnyMethod()  
                .AllowAnyHeader()  
                .AllowAnyOrigin();  
        });  
});

// app initialization
app.UseCors(corsOrigin);
```

---
