#Software-Engineering 

---
## # Explanation
In this node - i am gonna write about the story of our project `Eventify`

---
## # History:
---
## # --09.10.2023--
- Created Asp .NET Core Application with Blazor
- added MudBlazor Library
---

## # --16.10.2023--

- added NuGet packages: `MS.EntityFrameworkCore, MS.EntityFrameworkCore.Tools, MS.EntityFrameworkCore.Design, Pomelo.EntityFrameworkCore.MySql` 
  
- added models (Shared)
	- added new ClassLibrary to project - Eventify.Shared `Event.cs, User.cs, Category.cs` 

- added new Folder: DataBase
	- added AppDbContext.cs Class file - register AppDbContext as Service
	  
```csharp
// AppDbContext.cs
public class AppDbContext : DbContext  
{  
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }  
  
    public virtual DbSet<Event> Event { get; set; }  
    public virtual DbSet<User> User { get; set; }  
}

// Program.cs
string connectionString = builder.Configuration.GetConnectionString("ConnString");  
builder.Services.AddDbContext<AppDbContext>(options =>  
    options.UseMySql(connectionString, new MySqlServerVersion(new Version(10, 4, 27))));
```

- register ConnectionString
	- appsettings.json (added ConnectionString)
	  
```json
{  
  "ConnectionStrings": {  
    "ConnString" : "Server=localhost; Port=3306; Database=DB_Eventify; User=root; Password=root"  
  },  
    
  "Logging": {  
    "LogLevel": {  
      "Default": "Information",  
      "Microsoft.AspNetCore": "Warning"  
    }  
  },  
  "AllowedHosts": "*"  
}
```

- after the setup in JB Rider for the DB was completed - opened terminal + initialized DB
  
```terminal
// switch into folder where *.csproj is located
dotnet tool install --global dotnet-ef

dotnet ef migrations add Init  
dotnet ef database update
```

- now the DB should be connected
---

## # --17.10.2023--

- added Interfaces & Services
	- folder Interfaces
	- folder Services
	  
```csharp
// Interface - CRUD
namespace Eventify.Interfaces;  
  
public interface ICRUD<T> where T : class  
{  
    bool Add(T entity);  
    bool Update(T entity);  
    bool Delete(T entity);  
    T GetByID(int ID);  
    List<T> GetAll();  
}

// Class CRUD
using Eventify.DataBase;  
using Eventify.Interfaces;  
using Eventify.Shared;  
  
namespace Eventify.Services;  
  
public class ServiceBase<T> : ICRUD<T> where T:class  
{  
    // Instance of DBcontext  
    private readonly AppDbContext _context;  
    protected IQueryable<T> Entities  
    {        get => _context.Set<T>();  
    }  
    public ServiceBase(AppDbContext context)  
    {  
        this._context = context;  
    }  
    public bool Add(T entity)  
    {  
        try  
        {  
            _context.Set<T>().Add(entity);  
            return _context.SaveChanges() != 0;  
        }  
        catch  
        {  
            return false;  
        }  
    }  
    public bool Update(T entity)  
    {  
        try  
        {  
            _context.Set<T>().Update(entity);  
            return _context.SaveChanges() != 0;  
        }  
        catch  
        {  
            return false;  
        }  
    }  
    public bool Delete(T entity)  
    {  
        try  
        {  
            _context.Set<T>().Remove(entity);  
            return _context.SaveChanges() != 0;  
        }  
        catch  
        {  
            return false;  
        }  
    }  
    public T GetByID(int ID)  
    {  
        return _context.Set<T>().Find(ID);  
    }  
  
    public List<T> GetAll()  
    {  
        // gets all rows in Entity - adds it into a list  
        return _context.Set<T>().ToList();  
    }  
}
```

- register/build Service
  
```csharp
// Imports.razor
@using Eventify.Services  
@inject EventService eventService

// program.cs
builder.Services.AddTransient<EventService>();
```

----

## # --27.10.2023--

- modified: `NavMenu.razor`, `ManageEventPage.razor`
- created: `Dlg_AddEvent.razor`

---
## # --30.10.2023--

- modified: `ManageEventPage.razor`
- created: `EventComponent.razor`
---
## # --03.11.2023--

- modified: `ManageEventPage.razor`, `EventComponent.razor`
---
## # --08.11.2023
- modified: `ManageEventPage.razor`
- created: `SearchbarComponent.razor`
---
## # --20.11.2023--
- establish connection to the `Eventbrite.com/API`
- getting OAuth Token 
---
## # --21.11.2023--
- Testing with Eventbrite-API