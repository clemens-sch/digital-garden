#Software-Engineering 

---
## # What is a Controller?

In the context of Web-Dev, a Controller is a component of the Model-View-Controller (MVC) pattern.
Controllers are responsible for handling user input, processing it & updating the model/view accordingly.

A Controller acts as interface between the UI & application logic.

![[Pasted image 20240219100518.png]]

---
### # Controller-Types

#### # Api Controllers

Are specialized type of controllers designed for building APIs.
They inherit from `ControllerBase` and are decorated with the `[ApiController]` attribute.

Api-Controllers handle HTTP requests & return data, that a client can consume.

#### # MVC Controllers

#### # Web API Controllers

---
### # Dependency Injection

- [[SE_Dependency-Injection]]

#### # Constructor Injection

When you create an instance of a class, we specify which other parts of our code it should use directly during its creation.

This information is provided through the constructor. 
- constructor - (method called when you create a new instance of a class)

Here: an instance of IStudentRepository is injected

```csharp
public class StudentController : ControllerBase 
{ 
	private IStudentRepository studentRepository; 
	
	public StudentController(IStudentRepository studentRepository) 
	{ 
		this.studentRepository = studentRepository;
	} 
	
	// Other action methods... 
}
```

In the `Program.cs` the Service is registered & a new object of StudentRepository is created for each HTTP request within a scope.

```csharp
builder.Services.AddControllers();
builder.Services.AddScoped<IStudentRepository, StudentRepository>();
// maps controllers - to handle incoming HTTP requests
app.MapControllers();
```

