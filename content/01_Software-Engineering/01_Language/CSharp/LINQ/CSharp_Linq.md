#Software-Engineering 

[Main-Source](https://deep-thought.norwin.at/tech-kb/dotnet/Linq/)

---
# # Linq

Language Integrated Query

---
## # Linq - Basics

**Linq** allows querying data that are available in various different formats using a unified querying model.

We might query data in one of the following representations:

- Relational data using SQL
- In-Memory Data as object graphs
- XML documents
- …

Linq uses a _declarative_ programming paradigm. You define what you want, not how to get there.

---
## # Linq - Basics (2)

A Linq Query consists of 3 steps:

1. Define a data source
2. Create a query
3. Execute the query

---
## # Linq - Data Source

Linq heavily build on the <mark style="background: #D2B3FFA6;">IEnumerable</mark> and <mark style="background: #D2B3FFA6;">IQueryable</mark> interfaces. Whenever a data source implements one of these interfaces, Linq can be used to query the data behind it.

_IEnumerable_ - return element after element
_IQueryable_ - more powerful: allows to query & return element after element

If a datasource doesn’t offer one of these interfaces, the data must be loaded into memory (e.g. into a List, Array, or similar that implements IEnumerable) to be able to query it.

---
## # Linq - Data Source (2)

```csharp
// Step 1 - Define data source
List<Product> products = new List<Product> {
	new Product("Coke", categoryId: 0),
	new Product("Tea", categoryId: 0),
	new Product("Apple", categoryId: 1)
}
```
---
## # Linq - Create a query

Linq offers 2 different ways of creating a query

- Query Expressions
- Method Syntax
  
---
## # Query Expressions

Query expressions look similar to SQL statements. These expressions are built into the C# language and converted by the compiler to method calls.

```csharp
var query = 
	from product in products
	join category in categories on
		product.CategoryId equals category.Id
	// anonym class - creates new objects
	select new {
		CategoryName = category.CategoryName,
		Name = product.Name
	}
```

---
## # Query Expressions (2)

![LinqQueryExpression.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqQueryExpression.excalidraw.svg)

```csharp
var result 
// Datatype var: IEnumerable<string>
```
---
## # Method syntax

The same query can be defined using regular C# method calls and lambda expressions.

```csharp
var query = products
    .Join(
        categories, 
        p => p.CategoryId, 
        c => c.Id, 
        // result selector
        (p, c) => new
        {
            CategoryName = c.CategoryName,
            Name = p.Name
        });
```

---
## # Linq - Execute the query

Depending on the underlying implementation of the data source, executing the query might mean completely different things.

E.g. when querying data stored in memory (e.g. `List<Product>`) it will use C# methods to create the result. When talking to a database using an IQueryable object, an SQL query will be generated instead and executed on the database (that’s called **Linq to SQL** btw).

When talking to RAM it's called **Linq to Object**

---
## # Linq - Executing the query

You can execute a query by enumerating the elements of the IEnumerable interface, for example using a foreach loop, or by calling ToList:

```csharp
// Execute a query using a loop
foreach (var product in query) {
	Console.WriteLine(
		$"{product.Name} {product.CategoryName}");
}

// Execute a query using ToList
var result = query.ToList();
```

---
## # Deferred execution / Lazy evaluation

When defining a Linq query, no operation on the underlying data source will be triggered. We only specify what will happen, once we actually execute the query.

Only by enumerating the query, calling ToList / ToArray, or similar we actually perform the operations specified in the query.

---
## # Selection and Projection

![SelectionProjection.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/SelectionProjection.excalidraw.svg)

When using a where operation, what we do is actually a selection. We choose which of the available items should be included in the result.

When using a select operation, what we do is actually a projection. We project one representation of data on another representation. For example instead of querying projects, we only query the name for each project.

---
## # Linq - Select ( Projection)

![LinqSelect1.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqSelect1.svg)

---
## # Linq - Select (Projection) (2)

![LinqSelect2.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqSelect2.svg)

---
## # Linq - Select (Projection) (3)

For projection you can also use anonymous classes.

```csharp
IQueryable<Customer> customers = db.GetTable<Customers>();

var namePhoneQuery =
	from cust in customers
	where cust.City == "London"
	select new 
	{
		Name = cust.Name,
		Phone = cust.Phone
	};

// here the SQL query gets generated
foreach (var item in namePhoneQuery) {
	Console.WriteLine($"{item.Name}: {item.Phone}");
}
```

---
## # Linq - Where (Selection)

We can use the `where` clause in Linq to restrict the data we want to return. This is called selection, or filtering.

---
## # Linq - Where (Selection) (2)

```csharp
IList<Student> studentList = new List<Student>() {
	new Student
	{
		StudentId = 1,
		StudentName = "John",
		Age = 13
	},
	new Student 
	{
		StudentId = 2,
		StudentName = "Detlef",
		Age = 21
	}
};
```

---
## # Linq - Where (Selection) (3)

```csharp
// Query all students whose names have at least 5 characters
var result = from s in studentList
             where s.Length >= 5
             select s;

// Find all students aged between 13 and 19
var result = from s in studentList
             where s.Age <= 19 && s.Age >= 13
             select s;
```

---
## # Linq - Where with delegate

```csharp
// Func<> - generic delegate-datatype; <inputParameter;returnParameter>
Func<Student, bool> isTeenager =
	(Student s) => s.Age <= 19 && s.Age >= 13;

// Find all students aged between 13 and 19
var result = from s in studentList
             where isTeenager(s)
             select s;
```

---
## # Linq - From

Use **from** to define the data source of your query.

```csharp
var result = from s in studentList
             select s.StudentName.Substring(0, 1);
```

---
## # Linq - OrderBy

Use **OrderBy** to sort the elements of your query.

```csharp
// string[] array = [];
string[] words = [
	"the", "quick", "brows", "fox", "jumps"
];

IEnumerable<string> query = from word in words
                            orderby word.Length
                            select word;
```

---
## # Linq - OrderBy (2)

You can also define the order (ascending or descending). If not specified, ascending sorting will be used.

```csharp
string[] words = [
	"the", "quick", "brows", "fox", "jumps"
];

IEnumerable<string> query = from word in words
                            orderby word.Length descending
                            select word;
```

---
## # Data Aggregation

You can use different forms of join operations with Linq.

```csharp
from ... in <outerSequence>
join ... in <innerSequence>
on <outerKey> equals <innerKey>
select ...
```

---
## # Linq - Inner Join

```csharp
// Step 1: Define a data source
List<Product> products = new List<Product> {
	new Product("Apple", 1),
	new Product("Coke", 0),
	new Product("Tea", 0)
};

List<Category> categories = new List<Category> {
	new Category(0, "Beverage"),
	new Category(1, "Fruit"),
	new Category(2, "Vegetable")
};
```

---
## # Linq - Inner Join (2)

```csharp
// For all products, find the product name and category name
var result = 
	from p in products
	join c in categories
		on p.CategoryId equals c.Id
	// anonym type
	select new {
		CategoryName = c.CategoryName,
		Name = p.Name
	}
```

---
## # Linq - Inner Join - Object Association

```csharp
// Define a data source
class Person {
	public string FirstName { get; set; }
	public string LastName { get; set; }
}

class Pet {
	public string Name { get; set; }
	public Person Owner { get; set; }
}
```

---
## # Linq - Inner Join - Object Association (2)

```csharp
// For all pets, find the pet's name and the owner's name
var query =
	from p in people
	join pet in pets 
		on p equals pet.Owner
	select new {
		OwnerName = p.FirstName,
		PetName = pet.Name
	}
```

---
## # Linq - Inner Join - Composite Key

```csharp
// Define a data source
List<Employee> employees = new List<Employee> {
	new Employee(
		"Terry", "Adams", 522459
	)
};

List<Student> students = new List<Student> {
	new Student(
		"Vernette", "Price", 9562
	)
};
```

---
## # Linq - Inner Join - Composite Key (2)

```csharp
// Define a query
var result =
	from e in employees
	join s in students
	on new {
		e.FirstName, e.LastName
	}
	equals new {
		s.FirstName, s.LastName
	}
	select e.FirstName + " " + e.LastName;
```

---
## # Linq - Inner Join - Multiple Join

```csharp
// Define a data source
class Person {
	public string FirstName { get; set; }
	public string LastName { get; set; }
}

class Pet {
	public string Name { get; set; }
	public Person Owner { get; set; }
}

class Cat : Pet { }
class Dog : Pet { }
```

---
## # Linq - Inner Join - Multiple Join (2)

```csharp
// Define a query
var query =
	from p in people
	join c in cats
		on p equals c.Owner
	join d in dogs
		on new {
			Owner = p,
			Letter = c.Name.Substr(0, 1)
		}
		equals new {
			d.Owner, Letter = d.Name.Substr(0, 1)
		}
	select new {
		CatName = c.Name,
		DogName = d.Name
	}
```

---
## # Linq - Partitioning

Partitioning operators are used to **partition** the result of a query.

![LinqPartitioning.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqPartitioning.excalidraw.svg)

---
## # Linq - Partitioning (2)

- Operators for Partitioning:
    - Skip
    - SkipWhile
    - Take
    - TakeWhile

---
## # Linq - Partitioning (3)

**Skip** ignores the first n elements.

```csharp
IEnumerable<string> query = select ... ;

query.Skip(5);
```

---
## # Linq - Partitioning (4)

**SkipWhile** ignores elements at the beginning of the data source as long as they fulfil a certain condition.

```csharp
IEnumerable<string> query = {59, 82, 70, 56, 92};

query.SkipWhile(grade => grade >= 80);
```

---

[

## # Linq - Partitioning (5)

**Take** limits the result to a certain amount of elements.

```csharp
IEnumerable<string> query = {59, 82, 70, 56, 92};
query.Take(3);
```

---
## # Linq - Aggregation

**Aggregation Operators** can be used to find aggregates of multiple data entries.

![LinqAggregation.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqAggregation.excalidraw.svg)

---
## # Linq - Aggregation (2)

- Aggregation Operators:
    - Min, Max, Average
    - Count, Sum
    - Aggregate

---
## # Linq - Aggregation (3)

```csharp
// Compute the sum of the elements of an array
var numbers = new int[] { 4, 56, 2, 5, 43, 5 };
int sum = (from n in numbers).Sum();
```

```csharp
// Compute the sum of the elements of an array
var numbers = new int[] { 4, 56, 2, 5, 43, 5 };
int sum = numbers.Sum();
```

---
## # Linq - Aggregation (4)

```csharp
// Compute the sum of the elements of an array
var numbers = new int[] { 4, 5, 3, 9 }j

int sum = numbers.Aggregate(
	(result, item) => result + item
);
```

---
## # Linq - Aggregation (5)

![LinqAggregation2.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqAggregation2.excalidraw.svg)

---
## # Linq - Grouping

Use tho group clause to group data.

![LinqGroup.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqGroup.excalidraw.svg)

---
## # Linq - Grouping (2)

```csharp
List<int> numbers = new List<int> {
	35, 44, 200, 84, 3987, 4, 199, 446
};

// Group the list of numbers into even and odd numbers
IEnumerable<IGrouping<int, int>> query =
	from n in numbers group n by n % 2;
```

```csharp
List<int> numbers = new List<int> {
	35, 44, 200, 84, 3987, 4, 199, 446
};

// Group the list of numbers into even and odd numbers
// IGrouping<KEY, Datatype>
IEnumerable<IGrouping<int, int>> query =
	numbers.GroupBy(n => n % 2);
```

---
## # Linq - Grouping (3)

```csharp
foreach (var group in query) {
	Console.WriteLine(
		group.Key == 0
			? "Even numbers"
			: "Odd numbers")

	foreach (var i in group) {
		Console.WriteLine(i);
	}
}
```

---
## # Linq - Grouping (4)

```csharp
List<Student> students = new List<Student> {
	new {
		"Terry", "Adams", 120, GradeLevel.SecondYear
	},
	new {
		"Fadi", "Fakhouri", 120, GradeLevel.ThirdYear
	}
};
```

---
## # Linq - Grouping (5)

Group elements by a single property.

```csharp
var lastNames = 
	from s in students
	group s by s.LastName
	into newGroup // grouped data
	orderby newGroup.Key
	select newGroup;
```

```csharp
var lastNames = students
	.GroupBy(s => s.LastName)
	.OrderBy(g => g.Key);
```

---
## # Linq - Grouping (6)

Group elements by a derived value.

```csharp
var groupResults = 
	from s in students
	group s by s.LastName[0]
```

```csharp
var groupResults = students
	.GroupBy(s => s.LastName[0]);
```

```csharp
foreach (var studentGroup in groupResults) {
	foreach (var s in studentGroup) {
		Console.WriteLine(
			$"{s.LastName} {s.FirstName}"
		);
	}
}
```

---
## # Linq - Grouping (7)

```csharp
var query = 
	from s in students
	group s by new {
		FirstLetter = s.LastName[0],
		Score = s.ExamScore > 85
	}
	into studentGroup // grouped data
	orderby studentGroup.Key.FirstLetter
	select studentGroup;
```

```csharp
var query = students
	.GroupBy(s => new {
		FirstLetter = s.LastName[0],
		Score = s.ExamScore > 85
	})
	.OrderBy(g => g.Key.FirstLetter);
```

---
## # Linq - Group and Filter

```csharp
var query = 
	from s in students
	group s by new {
		FirstLetter = s.LastName[0],
		Score = s.ExamScore > 85
	}
	into studentGroup
	where studentGroup.Count() > 3
	select studentGroup
```

```csharp
var query = students
	.GroupBy(s => new {
		FirstLetter = s.LastName[0],
		Score = s.ExamScore > 85
	})
	.Where(g => g.Count() > 3)
	.OrderBy(g => g.Key.FirstLetter);
```

---
## # Linq - Min, Max, Average

```csharp
var players = new List<Player> {
	new Player(name: "Alex", team: "A", score: 10),
	new Player(name: "Anna", team: "A", score: 20),
	new Player(name: "Luke", team: "L", score: 60),
	new Player(name: "Lucy", team: "L", score: 40)
};
```

---
## # Linq - Min, Max, Average (2)

```csharp
var scores =
	from p in players
	group p by p.Team into playerGroup
	select new {
		Team = playerGroup.Key,
		TotalScore = playerGroup.Sum(
			x => x.Score
		)
	};
```

```csharp
var scores = players
	.GroupBy(p => p.Team)
	.Select(g => new {
		Team = playerGroup.Key,
		TotalScore = g.Sum(x => x.Score)
	});
```

---
## # Linq - Set Operations

To further specify the data source of a query you can use set operations such as:

- Distinct
- Except
- Intersect
- Union

---
## # Linq - Distinct

The **Distinct** method removes duplicates from the data source.

![LinqDistinct.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqDistinct.excalidraw.svg)

---
## # Linq - Distinct (2)

```csharp
string[] planets = {
	"Mercury", "Venus", "Venus", "Earth", "Mars", "Earth"
};
```

```csharp
var query =
	from planet in planets.Distinct()
	select planet;
```

```csharp
var query = planets.Distinct();
```

---
## # Linq - Except

![LinqExcept.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqExcept.excalidraw.svg)

---
## # Linq - Except (2)

```csharp
string[] planets1 = {
	"Mercury", "Venus", "Earth", "Jupiter"
};

string[] planets2 = {
	"Mercury", "Earth", "Mars", "Jupiter"
};
```

```csharp
var query =
	from planet in planets1.Except(planets2)
	select planet;
```

```csharp
var query = planets1.Except(planets2);
```

---
## # Linq - Intersect

Intersect finds elements that appear in both data sources.

![LinqIntersect.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqIntersect.excalidraw.svg)

---
## # Linq - Intersect (2)

```csharp
string[] planets1 = {
	"Mercury", "Venus", "Earth", "Jupiter"
};

string[] planets2 = {
	"Mercury", "Earth", "Mars", "Jupiter"
};
```

```csharp
var query =
	from planet in planets1.Intersect(planets2)
	select planet;
```

```csharp
var query = planets1.Intersect(planets2);
```

---
## # Linq - Union

Union combines two data sources to one, including all elements from both collections.

![LinqUnion.excalidraw.svg](https://deep-thought.norwin.at/tech-kb/dotnet/assets/LinqUnion.excalidraw.svg)

Union will also remove duplicates as long as the items can be compared using the overriden `Equals` method or an comparer object that implements the `IEquatable<T>` interface.

---
## # Linq - Union (2)

```csharp
string[] planets1 = {
	"Mercury", "Venus", "Earth", "Jupiter"
};

string[] planets2 = {
	"Mercury", "Earth", "Mars", "Jupiter"
};
```

```csharp
var query = 
	from planet in planets1.Union(planets2)
	select planet;
```

```csharp
var query = planets1.Union(planets2);
```

---
## # Linq - Quantifiers

Quantifiers classify a set of values. The return value of a quantifier is a **boolean value**.

- Operators:
    - All
    - Any
    - Contains

---
## # Linq - All

The **All** quantifier checks, whether each and everyone of the elements of a data source fulfils a condition.

```csharp
List<Market> markets = new List<Market> {
	new Market {
		Name = "Emiliy",
		Items = new string[]{"kiwi", "cherry", "banana"}
	},
	new Market {
		Name = "Kim",
		Items = new string[]{"melon", "mango", "olive"}
	}
};
```

---
## # Linq - All (2)

```csharp
// Which market only offers fruits 
// of which the name is at least 5 characters long
var query = 
	from m in markets
	where m.Items.All(item => item.Length >= 5)
	select m.Name;
```

```csharp
var query = markets
	.Where(m => m.Items.All(item => item.Length >= 5));
```

---
## # Linq - Any

The **Any** quantifier checks, whether at least one element fulfils a condition.

```csharp
// Which market sells at least one kind of fruit
// that starts with the letter o.
var query =
	from m in markets
	where m.Items.Any(item => item.StartsWith("o"))
	select m.Name;
```

```csharp
var query = markets
	.Where(m => m.Items.Any(item => item.StartsWith("o")));
```

---
## # Linq - Contains

The **Contains** quantifier checks whether a certain element exists in the data source.

```csharp
// Which market sells at least one fruit that starts with o
var query =
	from m in markets
	where m.Items.Contains("kiwi")
	select m.Name;
```

```csharp
var query f markets
	.Where(m => m.Items.Contains("kiwi"));
```

---
## # Linq - Subqueries

Subqueries are queries that are embedded into other queries.

```csharp
var players = new List<Player> {
	new Player(name: "Alex", team: "A", score: 10),
	new Player(name: "Anna", team: "A", score: 20),
	new Player(name: "Luke", team: "L", score: 60),
	new Player(name: "Lucy", team: "L", score: 40)
};
```

---
## # Linq - Subqueries (2)

```csharp
// Which players got the most points?
var query = 
	from p in players
	where p.Points ==
		// same as FROM Points p1 in ... SELECT p1.Points 
		(from p1.Points in players).Max()
	select p;
```

```csharp
var query = players
	.Where(p => p.Points == players.Max(p1 => p1.Points));
```