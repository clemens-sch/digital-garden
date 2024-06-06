#Software-Engineering 

---
## # Data Transfer Object

To manage data-transmission from 
- Client --> Server / Server --> Client

Data transfer objects - as the name implies - are objects that are used to transfer data.

Very often this means sending data over a network boundary - for example via a REST-API.

Data transfer objects solely hold data values. While classes or records that are used to implement DTOs can have additional functionality as methods, the main purpose is to (de-)serialize the data fields of DTOs so they can be transfered to its destination.

Usually DTOs don’t offer functionality that goes beyond storage, retrieval and (de-)serialization of their own data.

---
## # Implementation

Commonly DTOs are implemented as simple classes with only properties (POCOs) or as record types.

```csharp
public class Person {
	public string Name { get; set; }
}
// or
public record Person(string Name);
```

---
## # DTOs for REST API Design

DTOs can be very useful when designing REST APIs. Consider this data model:

![ErDiagram.png](https://deep-thought.norwin.at//tech-kb/web-development/assets/ErDiagram.png)

---
## # Entities in APIs

When using entities (for accessing and storing data) you might and up with something like this:

```csharp
public class Student {
	[Key]
	public int Id { get; set; }
	public string Name { get; set; }
	public string Email { get; set; }

	// Navigation Property
	public Collection<Grade> Grades { get; set; } = [];
}
```

---

When using the entity above in your REST API you might end up with a POST Endpoint that would require you to send data in this form when adding a new student:

```json
{
	"id": 1,
	"name": "Hugo",
	"email": "hugo@provider.com",
	"grades": []
}
```

This raises at least 2 questions:

- Why do I need to specify an ID for the student? Shouldn’t the server take care of that?
- Why do I need to specify a list of grades for that student when I just want to add a student without any grades yet.
