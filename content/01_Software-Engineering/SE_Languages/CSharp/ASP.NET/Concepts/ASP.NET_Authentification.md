#Software-Engineering 

[Tutorial](https://www.youtube.com/watch?v=tNzSuwV62Lw)
[Scaffold](https://learn.microsoft.com/de-de/aspnet/core/security/authentication/scaffold-identity?view=aspnetcore-8.0&tabs=visual-studio)

---
## Authentification Methods

- Basic Authentification
- JWT - JSON Web Token
- Cookie-Based
- OAuth

---
## "Terminverwaltung"

The program uses a custom authentification for managing users.

User credentials (username, password) 
- verifies user list retrieved from an API-Endpoint.

```csharp
// Users.cs
[Table("Users")]  
public class Users  
{  
    [Key]  
    [Column("username")]  
    public string username { get; set; }  
    [Column("password")]  
    public string password { get; set; }  
    [Column("Email")]  
    public string email { get; set; }  
    public virtual ICollection<UserHasEvent> UserHasEvent { get; set; } = new List<UserHasEvent>();  
}
```

```csharp
// UserStorage.cs (DTO)
namespace TerminVerwaltung;  
  
public class UserStorage  
{  
    public string Username { get; set; }  
}
```

```csharp
// AuthService.cs
public class AuthService  
{  
    private readonly HttpClient _httpClient;  
  
    public AuthService(HttpClient httpClient)  
    {  
        _httpClient = httpClient;  
    }  
  
    public async Task<bool> ValidateUserAsync(string username, string password)  
    {  
        var users = await _httpClient.GetFromJsonAsync<List<Users>>("api/user");  
        var user = users?.FirstOrDefault(u => 
	        u.username == username && u.password == password);  
        return user != null;  
    }  
}
```

```csharp
// Login.razor
<div class="centered-text">  
    <div class="login-container">
	    <input id="username" type="text" class="form-control" @bind="username" />
	    <input id="password" type="password" class="form-control" @bind="password" />
		<button class="btn-login-account" @onclick="Login">Login</button>
	</div>
</div>

@code{
	private string username;  
	private string password;  
	private bool loginFailed;

	private async Task Login() 
	{  
	    _httpClient.BaseAddress = new Uri("http://localhost:5088/");  
	    var authService = new AuthService(_httpClient);  
	    loginFailed = !await authService.ValidateUserAsync(username, password);  
	    if (!loginFailed)  
	    {  
	        userStorage.Username = username;  
	        NavigationManager.NavigateTo("/");  
	    }  
	}
}
```

```csharp
// MainLayout.razor
@inject UserStorage UserStorage  

<div class="page">  
    <div class="sidebar">  
        <NavMenu/>      
		    <main>
			...
            <span>@UserStorage.Username</span>            
            <a href="login" target="_blank">Login</a>  
        </div>  
        <article class="content px-4">  
            @Body  
        </article>  
    </main>
    ...
</div>  
```

---
## Demonstration

Login

![[Pasted image 20240604161930.png]]

Logged in

![[Pasted image 20240604162005.png]]

---
## .NET Identity

System, built for managing user authentification, authorization & user roles.
Seamless integration with ASP.NET Core.
- Making it a robust choice for managing user identities

This mechanism uses **Cookie-Authentification** (Session managed on Server).

![[Pasted image 20240604173824.png]]

Functionalities
- create, manage, authenticate users
- handle roles, claims, policies

---
## Implementation

Microsoft makes it simple for us.

 > New Project: Create Blazor Web App + Auth (Individual authentification)
 > 
 > Existing Project: Add scaffolded item: Identity

This creates a full out-of-the-box working authentification system.
- Predifined components
- NuGet-packages
- ...

---
