#Software-Engineering 

---
## # Records

In C# `Records` can be used to define reference data types, similar to classes.

The main purpose for record data types is to encapsulate data. While you can add methods to records, most of the time, they are only used with data fields.

---
## # Definition

To define a record you can use the following syntax.

```csharp
public record Person(
	string FirstName, 
	string LastName, 
	int Age);
```

This would be similar to defining a class that has three properties: FirstName, LastName and Age.

---
## # Usage

To create an instance you need to use the constructor that will be generated for each record type:

```csharp
var p = new Person("Hugo", "von Hofmannsthal", 55);
```

You can access individual properties of the record, just as you would with classes.

```csharp
var age = p.Age;
```

---
## # Difference to classes

When comparing records to classes, on the first look they seem very similar. The main difference that we can spot, is a much shorter form for declaring records.

However there are a few differences we should highlight.

---
## # Positional syntax for property definition

You can use positional parameters to declare properties of a record and to initialize the property values when you create an instance:

```csharp
public record Person(string FirstName, string LastName);

public static void Main()
{
    Person person = new("Nancy", "Davolio");
    Console.WriteLine(person);
    // output: Person { FirstName = Nancy, LastName = Davolio }
}
```

The compiler will create:

- A constructor
- Init-only properties for every property you defined in the list

---
## # Positional and normal syntax

You can also define properties as you are used to in classes.

```csharp
public record Person(string FirstName, string LastName)
{
    public int? Age { get; init; }
}

var p = new Person("Peter", "Lustig");
var p2 = new Person("Steve", "Jobs")
{
    Age = 56
};
```

---
## # Immutable by default

The properties we define for records are immutable by default. Immutable means that we cannot change their values after the record instance has been created.

```csharp
var p = new Person("Hugo", "von Hofmannsthal", 55);
p.Age = 27; // compiler error
```

---
## # Why immutability is important

1. Easier to understand
    
    When using immutable data types the developers can more easily understand what is happening. If we know that the values of a certain reference will never change, we needn’t worry about it. Compare this to a class with simple properties where it’s much more difficult to find out which property changed at which point in time and which part of code did the change.
    

---

2. Thread safety
    
    Types become thread-unsafe when different threads can **read and modify** data. The most common problem are race conditions.
    
    When a data type is immutable, we don’t run into this problem at all. Read-only access from different threads isn’t a problem. Therefore we don’t need to take special care for making a record thread-safe.
    

---

3. Efficient change detection
    
    When a record changes, it means that a new record is created and the old one is being replaced. So if we want to detect whether a record object has changed, we only need to compare their references instead of comparing (all) individual properties.
    

---
## # Value equality

If you don’t override or replace equality methods, the type you declare governs how equality is defined:

- For `class` types, two objects are equal if they refer to the same object in memory.
- For `record` types, two objects are equal if they are of the same type and store the same values.

Reference equality is required for some data models. For example,  [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/) depends on reference equality to ensure that it uses only one instance of an entity type for what is conceptually one entity. For this reason, records **are not** appropriate for use as entity types in Entity Framework Core.

---
## # Value equality example

```csharp
public record Person(
	string FirstName, 
	string LastName, 
	string[] PhoneNumbers);

public static void Main()
{
    var phoneNumbers = new string[2];
    Person person1 = new("Nancy", "Davolio", phoneNumbers);
    Person person2 = new("Nancy", "Davolio", phoneNumbers);
    Console.WriteLine(person1 == person2); // output: True

    person1.PhoneNumbers[0] = "555-1234";
    Console.WriteLine(person1 == person2); // output: True

    Console.WriteLine(
	    ReferenceEquals(person1, person2)); // output: False
}
```

---
## # Nondestructive mutation

As `record` types are immutable, we cannot change individual properties. If we still need to change properties, we have to create a new instance of the record, copying all property values and modifying those that need to change. There is a short form that helps us with that.

```csharp
public record Person(
	string FirstName, 
	string LastName, 
	int Age);
	
var p = new Person("Hugo", "von Hofmannsthal", 55);

var p2 = p with { Age = 27 };

var p3 = p with {};
Console.WriteLine(p == p3); // output: True
```

---
## # Built-in formatting for display

Record types have a compiler-generated `ToString` method that displays the names and values of public properties and fields. The `ToString` method returns a string of the following format:

```text
<record type name> { <property name> = <value>, ...}
```

for example

```csharp
var p = new Person("Hugo", "von Hofmannsthal", 55);
Console.WriteLine(p;

// Output:
// Person { FirstName = Hugo, LastName = von Hofmannsthal, Age = 55 }
```

---
## # Inheritance

A record can inherit from another record. However, a record can’t inherit from a class, and a class can’t inherit from a record.

The syntax for using inheritance is very similar to those for classes:

```csharp
public abstract record Person(
	string FirstName, 
	string LastName);
	 
public record Teacher(
	string FirstName, 
	string LastName, 
	int Grade) 
	: Person(FirstName, LastName);
```

---
## # Summary

Given the following record:

```csharp
public record Person(
	string FirstName, 
	string LastName, 
	int Age);
```

To create a class with a similar behavior you would have to do the following:

---

Create init-only properties and a constructor:

```csharp
public class Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public int Age { get; init; }

    public Person(string firstName, string lastName, int age)
    {
        FirstName = firstName;
        LastName = lastName;
        Age = age;
    }
}
```

---

Override the `Equals` and `GetHashCode` methods:

```csharp
public override bool Equals(object obj)
{
	if (obj is not Person other)
		return false;

	return FirstName == other.FirstName &&
               LastName == other.LastName &&
               Age == other.Age;
}

public override int GetHashCode()
{
	return HashCode.Combine(FirstName, LastName, Age);
}
```

---

Override the `ToString` method:

```csharp
public override string ToString()
{
	return $"Person {{ FirstName = {FirstName}, LastName = {LastName}, Age = {Age} }}";
}
```

---
## # Where to use records?

Due to their nature, records are often used for implementing **[[CSharp_DTO]]**
