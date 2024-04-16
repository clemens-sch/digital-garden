#Software-Engineering 
[Source](https://deep-thought.norwin.at/tech-kb/dotnet/Entity-Framework/)

---
# # What is Entity Framework Core?
## # Object Relational Mapping

- Object oriented programming languages encapsulate data in **objects**.
- Relational databases are based on the mathematical concept of **relational algebra**.
- This conceptual discrepancy is called **relational impedance mismatch**.

---
## # Object Relational Mapping (2)

**Object Relational Mapping** is a programming technique that converts between data available in relational databases and objects in object-oriented programming languages.

**Entity Framework Core** is an implementation of an ORM for dotnet.
**Dapper** also an ORM for dotnet.

---
## # Object Relational Mapping (3)

![OR_Mapping_Concept.png](https://deep-thought.norwin.at/tech-kb/dotnet/assets/OR_Mapping_Concept.png)

---
## # Basic Techniques

- **ORM maps classes to tables**.
- One object corresponds to one row in a database table.
- Properties correspond to columns of a table.
- The Identity of one object is defined by the **primary key** of a table.

---
## # Basic Techniques (2)

- Data entries and foreign keys will be transformed to objects and references.
- When writing to the database, this conversion will be performed in the reversed direction.

---
## # Entity

- An **Entity** is an object that can be stored into a database.
- To use an object as an entity, the object’s class must be registered within the **database context** of an application.
	- We have to register it in DbContext

---
## # Entity - Default Mapping

When no further information is given, Entity Framework will try to guess how to map an entity to the database (does mapping per default).

```csharp
public class Dish {
	public int Id { get; set; }
	public string Name { get; set; }
	public string Description { get; set; }
	public float Price { get; set; }
}
```

```sql
CREATE TABLE DISH (
	ID INT NOT NULL,
	NAME VARCHAR,
	DESCRIPTION VARCHAR,
	PRICE DECIMAL,
	PRIMARY KEY (ID)
)
```

---
## # Entity - Define Mapping

Entity Framework uses 2 techniques to define the mapping between entities and the database.

- Annotations (C# Attributes)
- Fluent API (C# Method calls)

---
## # Annotation - Table

By using the **Table** Annotation, you can specify the name of the table in the database.

```csharp
[Table("DISHES")]
[Comment("Dish Entity")] // optional
public class Dish {
	public int Id { get; set; }
	// ...
}
```

---
## # Annotation - Required

Properties that are marked with the **Required** attribute will get a **not null constraint** in the database.

```csharp
[Table("DISHES")]
public class Dish {
	public int Id { get; set; }

	[Required] // "not null" in db
	public string Name { get; set; }

	[Required]
	public string Description { get; set; }

	[Required]
	public float Price { get; set; }
}
```

---
## # Annotation - NotMapped

Properties marked with the **NotMapped** attribute, won’t be mapped to the database.

```csharp
[Table("DISHES")]
public class Dish {
	public int Id { get; set; }
	// ...
	[NotMapped]
	public DateTime LoadedFromDatabase { get; set; }
}
```

---
## # Annotation - Column

The **Column** attribute defines the column name that should be used for a property.

```csharp
[Table("DISHES")]
public class Dish {
	[Column("DISH_ID")]
	public int Id { get; set; }
	
	[Required]
	[Column("NAME", TypeName="VARCHAR(100)")]
	public string Name { get; set; }
	
	[Column("DESCRIPTION", TypeName="VARCHAR(255)")]
	public string Description { get; set; }
	
	[Required]
	[Column("PRICE", TypeName="DECIMAL(5,2)")]
	public float Price { get; set; }
}
```

---
## # Annotation - Key

The **Key** attribute marks a column as primary key.

By default a property that is named **Id** or **Id** prefixed by the **Classname** (e.g. DishId) will be used as primary key.

```csharp
[Table("DISHES")]
public class Dish {
	[Column("DISH_ID")]
	[Key]
	[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
	public int Id { get; set; }
	// ...
}
```

---

```csharp
[Table("DISHES")]
public class Dish {
	[Column("DISH_ID")]
	[Key]
	// says, how the PK should be created
	[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
	public int Id { get; set; }

	// "StringLength(...)" - string gets controlled directly in programm - exception
	[Required, StringLength(100)]
	[Column("NAME")]
	public string Name { get; set; }

	[Required]
	[Column("PRICE", TypeName="DECIMAL(5,2)")]
	public float Price { get; set; }
}
```

```SQL
CREATE TABLE DISH (
	DISH_ID INT NOT NULL AUTO INCREMENT,
	NAME VARCHAR(100) NOT NULL,
	PRICE DECIMAL(5,2) NOT NULL,
	PRIMARY KEY (DISH_ID)
)
```

---
## # DbContext

The **DbContext** is the central interface to communicate with the database when using Entity Framework.

The **DbContext** class is used to resolve the _Relational impedance mismatch_.

---
## # DbContext (2)

![DbContext.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/DbContext.excalidraw.svg)

---
## # DbContext - Registering Entities

- Classes that should be mapped to the database must be **registered** first.
- Registering happens by specifying a property of type **DbSet** for each class that should be mapped.

```csharp
public class CookbookContext : DbContext {
	// Registering a class as entity
	public DbSet<Dish> Dishes { get; set; }

	public CookbookContext(
		DbContextOptions<CookbookContext> options) 
		: base(options) {
		// ...
	}
}
```

---
## # DbContext - Configuring Entities

The **DbContext** can also be used to configure entities. Configuring means to define the mapping.

Entities can be defined completely in the DbContext without using Annotations at all.

---
## # DbContext - Fluent API

FluentAPI - uses Method chaining

```csharp
public class CookbookContext : DbContext {
	// ...
	protected override void OnModelCreating(
		ModelBuilder builder) {
		builder.Entity<Dish>()
			.ToTable("DISHES")
			.Property(d => d.Name)
			.HasColumnName("TITLE")
			.HasColumnType("VARCHAR(50)")
			.IsRequired();

		builder.Entity<Dish>()
			.Property(d => d.Description)
			// ...
	}
}
```

---
## # DbContext - Fluent API (2)

```csharp
public class CookbookContext : DbContext {
	protected override void OnModelCreating(
		ModelBuilder builder) {
		builder.Entity<Dish>()
			.HasKey(d => d.Id)
			.ValueGeneratedOnAdd();
	}
}
```

---
## # DbContext - Fluent API (3)

```csharp
public class CookbookContext : DbContext {
	protected override void OnModelCreating(
		ModelBuilder builder) {
		builder.Entity<Dish>()
			.HasIndex(d => d.Name)
			.IsUnique(true);
	}
}
```

---
## # ORM - Advanced Concepts

**ORM** bridges the gap between relational databases and object-oriented programming.

Some concepts from object-oriented design must be considered specifically for the mapping.

- Inheritance
- Object References
- Composite keys

---
## # ORM - Inheritance

Class inheritance is used in object-oriented design to define entities with similar properties in a hierarchical structure.

The relational design defines several different forms of inheritance.

![Inheritance.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/Inheritance.excalidraw.svg)

---
## # ORM - Single Table Inheritance

**Single Table Inheritance** saves all values of all (base and derived) entities into a single table.

This is also called **Table-per-hierarchy** configuration and is the default mapping strategy for inheritance when using Entity Framework Core.

---

![SingleTableInheritance.png](https://deep-thought.norwin.at/tech-kb/dotnet/assets/SingleTableInheritance.png)

---

```csharp
[Table("PERSONEN")]
public class Person {
	[Key]
	[Column("OID")]
	public int Id { get; set; }

	[Required, StringLength(100)]
	[Column("VORNAME")]
	public string FirstName { get; set; }

	// ...
}
```

---

```csharp
public class Student : Person {
	[Column("KAT_NR")]
	public int Code { get; set; }

	[Required, StringLength(4)]
	[Column("KLASSE")]
	public string ClassCode { get; set; }
}
```

```csharp
public class Teacher : Person {
	[Required, StringLength(4)]
	[Column("CODE")]
	public string Code { get; set; }

	[Required, StringLength(2)]
	[Column("KUERZEL")]
	public string Short { get; set; }
}
```

---

```csharp
public class UniversityContext : DbContext {
	protected override void OnModelCreating(
		ModelBuilder builder) {
		builder.Entity<Person>()
			.HasDiscriminator<string>("TYP")
			.HasValue<Student>("SCHUELER")
			.HasValue<Teacher>("LEHRER");
	}		
}
```

---
## # ORM - Joined Table Inheritance

The object data will be stored in a distributed way in multiple tables.

This is also called **Table-per-type** configuration.

---

![JoinedTableInheritance.png](https://deep-thought.norwin.at/tech-kb/dotnet/assets/JoinedTableInheritance.png)

---

Each class has a Table Annotation - so, EF knows, that every Type should be an own Entity.

```csharp
[Table("PERSONEN")]
public class Person {
	[Key]
	[Column("OID")]
	public int Id { get; set; }
	// ...
	public string FirstName { get; set; }
	// ...
}
```

```csharp
[Table("SCHUELER")]
public class Student : Person {
	// ...
	public int Code { get; set; }
	// ...
	public string ClassCode { get; set; }
}
```

```csharp
[Table("LEHRER")]
public class Teacher : Person {
	// ...
	public string Code { get; set; }
	// ...
	public string Short { get; set; }
}
```

---
## # ORM - Table-per-concrete-Type Inheritance

In the TPC mapping pattern, all the types are mapped to individual tables. Each table contains columns for all properties on the corresponding entity type. This addresses some common performance issues with the TPT strategy.

---

```csharp
public abstract class Animal
{
    protected Animal(string name)
    {
        Name = name;
    }

    public int Id { get; set; }
    public string Name { get; set; }
    public abstract string Species { get; }

    public Food? Food { get; set; }
}

public abstract class Pet : Animal
{
    protected Pet(string name)
        : base(name)
    {
    }

    public string? Vet { get; set; }

    public ICollection<Human> Humans { get; } = new List<Human>();
}

public class FarmAnimal : Animal
{
    public FarmAnimal(string name, string species)
        : base(name)
    {
        Species = species;
    }

    public override string Species { get; }

    [Precision(18, 2)]
    public decimal Value { get; set; }

    public override string ToString()
        => $"Farm animal '{Name}' ({Species}/{Id}) worth {Value:C} eats {Food?.ToString() ?? "<Unknown>"}";
}

public class Cat : Pet
{
    public Cat(string name, string educationLevel)
        : base(name)
    {
        EducationLevel = educationLevel;
    }

    public string EducationLevel { get; set; }
    public override string Species => "Felis catus";

    public override string ToString()
        => $"Cat '{Name}' ({Species}/{Id}) with education '{EducationLevel}' eats {Food?.ToString() ?? "<Unknown>"}";
}

public class Dog : Pet
{
    public Dog(string name, string favoriteToy)
        : base(name)
    {
        FavoriteToy = favoriteToy;
    }

    public string FavoriteToy { get; set; }
    public override string Species => "Canis familiaris";

    public override string ToString()
        => $"Dog '{Name}' ({Species}/{Id}) with favorite toy '{FavoriteToy}' eats {Food?.ToString() ?? "<Unknown>"}";
}

public class Human : Animal
{
    public Human(string name)
        : base(name)
    {
    }

    public override string Species => "Homo sapiens";

    public Animal? FavoriteAnimal { get; set; }
    public ICollection<Pet> Pets { get; } = new List<Pet>();

    public override string ToString()
        => $"Human '{Name}' ({Species}/{Id}) with favorite animal '{FavoriteAnimal?.Name ?? "<Unknown>"}'" +
           $" eats {Food?.ToString() ?? "<Unknown>"}";
}
```

---

```sql
CREATE TABLE [Cats] (
    [Id] int NOT NULL DEFAULT (NEXT VALUE FOR [AnimalSequence]),
    [Name] nvarchar(max) NOT NULL,
    [FoodId] uniqueidentifier NULL,
    [Vet] nvarchar(max) NULL,
    [EducationLevel] nvarchar(max) NOT NULL,
    CONSTRAINT [PK_Cats] PRIMARY KEY ([Id]));

CREATE TABLE [Dogs] (
    [Id] int NOT NULL DEFAULT (NEXT VALUE FOR [AnimalSequence]),
    [Name] nvarchar(max) NOT NULL,
    [FoodId] uniqueidentifier NULL,
    [Vet] nvarchar(max) NULL,
    [FavoriteToy] nvarchar(max) NOT NULL,
    CONSTRAINT [PK_Dogs] PRIMARY KEY ([Id]));

CREATE TABLE [FarmAnimals] (
    [Id] int NOT NULL DEFAULT (NEXT VALUE FOR [AnimalSequence]),
    [Name] nvarchar(max) NOT NULL,
    [FoodId] uniqueidentifier NULL,
    [Value] decimal(18,2) NOT NULL,
    [Species] nvarchar(max) NOT NULL,
    CONSTRAINT [PK_FarmAnimals] PRIMARY KEY ([Id]));

CREATE TABLE [Humans] (
    [Id] int NOT NULL DEFAULT (NEXT VALUE FOR [AnimalSequence]),
    [Name] nvarchar(max) NOT NULL,
    [FoodId] uniqueidentifier NULL,
    [FavoriteAnimalId] int NULL,
    CONSTRAINT [PK_Humans] PRIMARY KEY ([Id]));
```

---

---
## # ORM - 1:1 Relation

A 1:1 relation exists between 2 entities, if one of the entities has a reference to the other entity.

In a 1:1 Relation - the foreign key is unique.

```csharp
[Table("STUDENTS")]
public class Student : Person {
	// ...
	public int Code { get; set; }
	// ...
	public string ClassCode { get; set; }
}
```

```csharp
[Table("MATRICULATION_CARDS")]
public class MatriculationCard {
	[Key]
	[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
	public int Id { get; set; }

	// foreign key (saved in database)
	[Column("STUDENT_ID")]
	public int StudentId { get; set; }
	// navigation property
	public Student Student { get; set; }
}
```

---

```csharp
public class UniversityContext : DbContext {
	protected override void OnModelCreating(
		ModelBuilder builder) {
		// you start on the side with the fk
		builder.Entity<MatriculationCard>()
			// first side - "Student" = navigation property
			.HasOne(m => m.Student)
			// the other side
			.WithOne()
			.HasForeignKey<MatriculationCard>(m =>
				m.StudentId);
	}
}
```

---
## # ORM - Inheritance Summary

- In summary, TPH is usually fine for most applications, and is a good default for a wide range of scenarios, so don’t add the complexity of TPC if you don’t need it. Specifically, if your code will mostly query for entities of many types, such as writing queries against the base type, then lean towards TPH over TPC.
    
- That being said, TPC is also a good mapping strategy to use when your code will mostly query for entities of a single leaf type and your benchmarks show an improvement compared with TPH.
    
- Use TPT only if constrained to do so by external factors.
    

---
## # ORM - 1:n Relation

on the n side we need the foreign key.

![OneToManyRelation.png](https://deep-thought.norwin.at/tech-kb/dotnet/assets/OneToManyRelation.png)

---

```csharp
[Table("CLASS_ROOMS")]
public class ClassRoom {
	[Column("ROOM_CODE"), StringLength(4)]
	[Key]
	public string ClassId { get; set; }

	public ICollection<Student> Students { get; set; }
}
```

```csharp
[Table("SCHUELER")]
public class Student {
	[Column("STUDENT_ID")]
	[Key]
	[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
	public int Id { get; set; }
	// ...
	public ClassRoom Room { get; set; }
	[Column("CLASS_CODE")]
	public int ClassCode { get; set; }
}
```

---

```csharp
public class UniversityDbContext : DbContext {
	protected override void OnModelCreating(
		ModelBuilder builder) {
		builder.Entity<Student>()
			.HasOne(s => s.Room)
			.WithMany()
			.HasForeignKey(s => s.ClassCode);
	}
}
```

---
## # ORM - m:n Relation

![ManyToManyRelation.png](https://deep-thought.norwin.at/tech-kb/dotnet/assets/ManyToManyRelation.png)

---

```csharp
[Table("CLASS_ROOMS")]
public class ClassRoom {
	[Column("KLASSE")]
	[Key]
	public string ClassId { get; set; }

	[Required, StringLength(3)]
	[Column("RAUM")]
	public string RoomCode { get; set; }
}
```

```csharp
[Table("KLASSE")]
public class Teacher {
	[Column("SVNR")]
	[Key]
	public int SocialSecurity { get; set; }

	[Required, Range(0, 200)]
	[Column("ALTER")]
	public int Age { get; set; }
}
```

---

```csharp
[Table("TIME_TABLES_JT")]
public class LectureAssignment {
	[Column("SVNR")]
	public int SocialSecurity { get; set; }
	public Teacher Teacher { get; set; }

	[Column("CLASS_ROOM_CODE")]
	public string ClassId { get; set; }
	public ClassRoom ClassRoom { get; set; }

	[Required, StringLength(20)]
	[Column("COURSE")]
	public string Course { get; set; }
}
```

---

```csharp
public UniversityContext : DbContext {
	protected void OnModelCreated(ModelBuilder builder) {
		builder.Entity<LectureAssignment>()
			.HasKey(l => new { 
				l.SocialSecurity, l.ClassId
			});

		builder.Entity<LectureAssignment>()
			.HasOne(l => l.Teacher)
			.WithMany()
			.HasForeignKey(l.SocialSecurity);

		builder.Entity<LectureAssignment>()
			.HasOne(l => l.ClassRoom)
			.WithMany()
			.HasForeignKey(l.ClassId);
	}
}
```

---
## # Layers / Tiers

We typically build up our applications in layers. (Tier is just a synonym for layer.)

Each layer is responsible for one specific purpose. Splitting up responsibilities is also often referred to as **separation of concerns**.

---

One layer only communicates with the layers right next to itself, but never skips a layer.

![LayeredArchitecture.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LayeredArchitecture.excalidraw.svg)

---
## # 3 Tier Architecture

The 3 tier architecture is one of the most widely used approaches in how to structure software.

The three layers used in this architecture are as follows.

![ThreeTierArchitecture.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/ThreeTierArchitecture.excalidraw.svg)

---
## # Data Access Layer

- Synonyms:
    - DAL
    - Model
    - Data Layer

Responsible for accessing (reading/writing) data. Very often this means accessing a database, but how to persist data is not standardized. You could have a data access layer that stores data to files just as well. When using C# the data access layer often is implemented using an ORM like Entity Framework.

---
## # Business Logic Layer

- Synonyms:
    - BL
    - Logic
    - Domain

This layer is responsible for processing information. We work on a higher level than accessing individual database tables. Additionally, to using the DAL for accessing single tables, the BL can contain more complex operations including multiple datasets and other systems such as an e-mail service.

Example:
- a method for registering the user in a DB
- then, the user should receive a verification Mail

---
## # Presentation Layer

- Synonyms:
    - View
    - Service Layer

The presentation layer presents the information to the user and offers possibilities to modify it. Very often, the presentation layer will be a (web-) application. In a way, a simple API, such as a REST-API, can also be considered a presentation layer in the context of a three-tier architecture.

---
## # Domain Layer - Repository

The `repository pattern` is a widely used way to structure your code to get reusable functionality to access data.

The `IRepository` interface connects the presentation layer and the domain layer.

---
## # IRepository Pattern

The `IRepository` is a generic interface. This means it can be implemented for different entities. Users of the IRepository might want to use a `IRepository<Teacher>` or a `IRepository<Pupil>` to access either teacher data or pupil data.

```csharp
public interface IRepository<TEntity> 
	where TEntity : class {
	// ...
}
```

---
## # IRepository - Create

!! Note: all classess here are sync over async !!

```csharp
public interface IRepository<TEntity> 
	where TEntity : class {
	
	// Create a new entity - returns it, we can inspect the id
	TEntity Create(TEntity t);

	// Create multiple entities
	List<TEntity> CreateRange(List<TEntity> list);
}
```

---
## # IRepository - Update

```csharp
public interface IRepository<TEntity> 
	where TEntity : class {
	
	// Modify an entity
	void Update(TEntity t);

	// Modify multiple entities
	void UpdateRange(List<TEntity> list);
}
```

---
## # IRepository - Read

```csharp
public interface IRepository<TEntity> 
	where TEntity : class {

	// "?" - nullable - when it does not exist in the DB
	TEntity? Read(int id);

	// like a filter - should the Element beeing within or not
	// Expression - EF needs this - then it can build SQL-Statements - "Expression Trees"
	List<TEntity> Read(
		Expression<Func<TEntity, bool>> filter
	);

	// "Pagination" - you don't load all DB-entries
	List<TEntity> Read(int start, int count);

	// don't use this - could lead to performance issues
	List<TEntity> ReadAll();
}
```

---
## # IRepository - Delete

```csharp
public interface IRepository<TEntity> 
	where TEntity : class {
	void Delete(TEntity t);
}
```

---
## # ARepository

Instead of implementing all the interface methods for each entity, we use an abstract base class where we implement all these functionalities once, that doesn’t differ between different entities.

```csharp
public abstract class ARepository<TEntity>
	: IRepository<TEntity> where TEntity : class {
	protected DbContext _context;
	protected DbSet<TEntity> _table;

	protected ARepository(DbContext context) {
		_context = context;
		_table = context.Set<TEntity>();
	}
}
```

---
## # ARepository - Create

```csharp
public abstract class ARepository<TEntity>
	: IRepository<TEntity> where TEntity : class {
	
	public TEntity Create(TEntity t) {
		_table.Add(t);
		_context.SaveChanges();
		return t;
	}

	public List<TEntity> CreateRange(List<TEntity> list) {
		_table.AddRange(list);
		_context.SaveChanges();
		return list;
	}
}
```

---
## # ARepository - Update

```csharp
public abstract class ARepository<TEntity>
	: IRepository<TEntity> where TEntity : class {
	
	public void Update(TEntity t) {
		// State.Modified = .Update()
		_table.Update(t);
		_context.SaveChanges();
	}

	public void UpdateRange(List<TEntity> list) {
		// State.Modified = .Update()
		_table.UpdateRange(list);
		_context.SaveChanges();
	}
}
```

---
## # ARepository - Read

```csharp
public abstract class ARepository<TEntity>
	: IRepository<TEntity> where TEntity : class {
	
	public List<TEntity> Read(
		Expression<Func<TEntity, bool>> filter) 
		// .Where - wants YES/NO
		=> _table.Where(filter).ToList();

	public List<TEntity> Read(int start, int count)
		=> _table.Skip(start)
			.Take(count)
			.ToList();

	public List<TEntity> ReadAll() => _table.ToList();
}
```

---
## # ARepository - Delete

```csharp
public abstract class ARepository<TEntity>
	: IRepository<TEntity> where TEntity : class {
	
	public void Delete(TEntity t) {
		_table.Remove(t);
		_context.SaveChanges();
	}
}
```

---
---