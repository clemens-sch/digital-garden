#Software-Engineering 

---
## # What is Entity Framework Core?

It is a database interaction framework (by .NET), mapping code to databases for simplified data access.
`Entity Framework Core (EF Core)` is an _Object-Relational Mapper (ORM)_.

So, developers don't have to write any SQL-statements manually.

---
## # DB-Migration: How it works

It is a structured way of evolving the db over time, highlighting changes in application's data model.

**How a db-migration works**

1. Create/Modify entity classes
	1. changes to data model - changes represent how data is structured in application.
	   
2. generate migration files
	1. devs use migration tool (EF Core) to generate migration files based on changes made to the entity class.
	2. these migration files contain instructions for updating the db schema.
	   
3. Review & customize migration files
	1. devs can review & check these automatically generated files.
	2. devs can customize migration files if needed.
	   
4. Apply migrations
	1. devs use migration tool to apply changes to db. This process executes SQL-statements for deleting, creating, modifying db-objects
	   
5. Update db
	1. migration tool updates a specific table in db
	   
The dev describes any entity classes. Changes represent how data is structured in the application.

---
### # Prompts: Add Migration; Update DB

```terminal
// add migration + cd into Entity directory
dotnet ef migrations add MigrationName
dotnet ef migrations add AnotherMigration -s ..\Timetable.Api\  

// update db
dotnet ef database update
dotnet ef database update -s ..\Timetable.Api\ 
```

---
## # : DbContext

Key component for an application to communicate with a db properly.
It represents a session with the db.

The `DbContext` class is a part of the DbContext API, which is a set of APIs for interacting with a db using a high-level, object-oriented approach.

### # How to use DbContext

```csharp
// create custom DbContext class - derives from DbContext
public class MyDbContext : DbContext
{
	public DbSet<User> Users { get; set; } 
}

// Define Entities
public class User
{
	public int UserId { get; set; }
}

// Configure DbContext
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
	// configure relationships, constraints, ...
}

// Modify db connection (not optimal method: defining connection string!)
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)   
{  
    base.OnConfiguring(optionsBuilder);
	// connection string
   optionsBuilder.UseNpgsql(@"Host=localhost;Username=root;Password=root;Database=db"); 
}
```


This creates some SQL-statements.
You can review these statements by adding this line in OnConfiguring()

```csharp
optionsBuilder.LogTo(Console.WriteLine);
```

---
## # DbSet<...>

Mapping between application's entity & corresponding table in db. 
We tell EF, that the User class corresponds to a db table.

```csharp
public class MyDbContext : DbContext
{
	public DbSet<User> Users { get; set; } 
}
```

The DbSet allows us to do db operations on the Users property (CRUD)

---
### # Code First Principle

In the context of EF Core:

1. **Code definition**: (in code) devs start by defining classes to represent entities (objects)
   
2. **attributes**: (in code) details such as table names, primary keys, relationships are specified
   
3. **DB generation**: db schema is generated automatically using ORM framework.
	1. Based on **code definition**
	2. known as "DataBase Migration"
	   
4. **Evolution of DB**: as app evolves - data model might change
	1. devs update **code definition**
	2. use migration tools to update db accordingly

---
---
