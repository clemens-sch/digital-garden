#Software-Engineering 

[[API_REST]]
[[SE_Dependency-Injection]]
[[Blazor_StateManagement]]

---
## # HttpClient

We gonna use the HttpClient class in combination with some methods to call REST-Endpoints.
Make sure to inject the HttpClient from the DI container.

```csharp
// comp.razor
@inject HttpClient Http
@inject NavigationManager NavigationManager
@rendermode InteractiveServer // when interaction happens on client-side
```

```csharp
// program.cs
builder.Services.AddScoped(sp => new HttpClient  
{  
    BaseAddress = new Uri("http://localhost:5217")  
});
```

---
## # GET 

### # GetFromJsonAsync<...>()

Let's say we want to load subjects from a REST-Api. We save them temporarely in a list.

```csharp
@page = "/subjects"
...

@code{
	private List<Subject>? subjects = [];

	protected override async Task OnInitializedAsync()
	{
		subjects = await Http.GetFromJsonAsync<List<Subject>>("Subject") ?? [];
	}
}
```

---
## # DELETE

### # .DeleteAsync()

Let's say we want to delete any subject (specific id) using a REST-Api. 

```csharp
@page = "/subjects/{id}"
@inject NavigationManager NavigationManager
@rendermode InteractiveServer

@if(subject is not null)
{
	<EditForm ...>
		<label ..>
			<InputText .../>
		</label>
		...
	</EditForm>
}
<button @onclick="DeleteSubject">Delete</button>
...

@code{
	[Parameter] public string id { get; set; }
	private Subject? subject;

	private async Task DeleteSubject()
	{
		await Http.DeleteAsync($"Subject/{id}");
		NavigationManager.NavigateTo("Subjects");
	}
}
```

---
## # POST

### # .PostAsJsonAsync()

Let's say we want to add another subject using a REST-Api.

```csharp
@page = "/SubjectsAdd"
@inject NavigationManager NavigationManager
@rendermode InteractiveServer

<EditForm Model="subject" OnSubmit="AddSubject" FormName="AddSubject">
	<div>
		<label> 
			Id:
			<InputText @bind-Value="subject.Id"/>
		</label>
		<label> 
			Description:
			<InputText @bind-Value="subject.Description"/>
		</label>
	</div>
	<div>
		<button type="submit">Add</button>
	</div>
</EditForm>

@code{
	private Subject? subject = new();

	private async Task AddSubject()
	{
		await Http.PostAsJsonAsync("Subject", subject);
		NavigationManager.NavigateTo("Subjects");
	}
}
```

---
## # PUT

### # .PutAsJsonAsync()

Let's say we want to update a single subject (specific id) using our REST-Api.

```csharp
@page = "/subjects/{id}"
@inject NavigationManager NavigationManager
@rendermode InteractiveServer

<EditForm Model="subject" OnSubmit="OnSubmit" FormName="EditSubject">
	<div>
		<label> 
			Id:
			<InputText @bind-Value="subject.Id"/>
		</label>
		<label> 
			Description:
			<InputText @bind-Value="subject.Description"/>
		</label>
	</div>
	<div>
		<button type="submit">Submit</button>
	</div>
</EditForm>

@code{
	[Parameter] public string id { get; set; }
	private Subject? subject;

	protected override async Task OnParametersSetAsync()
	{
		subject = await Http.GetFromJsonAsync<Subject>("Subject/{id}");
	}

	private async Task OnSubmit()
	{
		await Http.PutAsJsonAsync($"Subject/{id}", subject);
		subject = await Http.GetFromJsonAsync<Subject>("Subject/{id}");
		NavigationManager.NavigateTo("Subjects");
	}
}
```

---