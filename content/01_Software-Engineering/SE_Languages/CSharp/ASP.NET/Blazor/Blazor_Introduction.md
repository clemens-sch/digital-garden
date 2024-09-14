#Software-Engineering 

[Main-Source](https://deep-thought.norwin.at/tech-kb/web-development/Blazor/)

---
# # Blazor
## # Client - Server

Web Applications consist of a server-component and a client-component.

![ClientServer.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/ClientServer.png)

---
## # Client - Server (2)

The business logic of a web application is primarily implemented on the server.

Clients are primarily used for presenting data and for user interaction.

---
## # Frontend - Backend

Server components are also commonly referred to as **Backend**.

Client components are also commonly referred to as **Frontend**.

---
## # Categorizing Blazor

Blazor is a **Frontend**-technology for the ASP.NET Core Framework.

Both Blazor and ASP.NET Core are created and maintained by Microsoft. Whereas ASP.NET Core is for creating web applications in general, Blazor fills the role of creating frontend applications within the ASP.NET Core ecosystem.

---
## # Frontend technologies

Basic technologies that are used to create web frontends are **Javascript, HTML and CSS**.

- **HTML**
    - is the structure of the UI.
- **CSS**
    - is for styling the elements defined with HTML.
- **JavaScript**
    - is for adding dynamic logic using a programming language.

---
## # Blazor SSR

Blazor in .NET 8 uses primarily server-side rendering (SSR) to deliver websites.

When the client requests a page, an HTTP request is sent to the server. The server calculates what needs to be rendered according to the client’s request and sends back HTML, CSS and JavaScript files to the client, where they will be presented to the user.
- C# Code is rendered in the server

---
## # Blazor SSR (2)

When using SSR with Blazor, you are very limited in reacting to user interaction.

For every action the user performs on the client, a request needs to be sent to the server, where the server processes the information and sends back the response as a new page, consisting of HTML, CSS and JavaScript.

---
## # Blazor User Interaction

When you want to allow more sophisticated forms of user interaction and (partial) view updates without the need to perform a server request in between you need to choose one of the following:

- Implement client logic with **JavaScript**
- Use **InteractiveServer** render mode
- Use **InteractiveWebAssembly** render mode

---
## # JavaScript Client logic

You can of course add JavaScript files to a Blazor project and implement dynamic logic on the client with it.

However, if you wanted to use JavaScript from the beginning, Blazor is probably not the correct technology for you to use. (Choose Angular, React, Vue, jQuery, … instead)

Blazor is primarily meant as a technology that uses C# for implementing client logic.

---
## # WebAssembly

WebAssembly is a special **Bytecode** that can be executed inside the browser.
- Bytecode - C# (IL - Intermediate Language) - compiler transforms original code into IL after this C# Runtime compiles it into machine code
- Bytecode is near machine code - can be translated easily into machine code

Together with **JavaScript**, these two languages are the only ones understood by the browser.

A **Bytecode** is usually not written by humans, but compiled from a higher programming language.

---
## # WebAssembly (2)

"Compile" - translate a language into machine code.
"Cross-compilation" - basic example: translating Js into Ts

Using WebAssembly it is possible to convert, or (cross-)compile the source code of a higher programming language (Java, C, C++, C#) and execute them in the browser.

**Bytecode** shouldn’t be confused with machine code. Machine code is directly executed on a processor, whereas **Bytecode** is interpreted by an interpreter.

---
## # Webassembly vs. TypeScript

![TypescriptWebassembly.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/TypescriptWebassembly.png)

---
## # Blazor WASM

".cshtml" - could also be ".razor"
DOM (Document Object Model) - elements, that get shown on the website
- follows a Tree-Structure

![HowBlazorWorks.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/HowBlazorWorks.png)

---
## # Blazor WASM

Blazor ships a WASM-compiled .NET Runtime to the browser.

It then sends the compiled razor views and dependencies as dll files to the browser. The browser can execute the code inside by using the .NET Runtime.

We effectively taught the browser how to understand and execute C# code.

---
## # Blazor Server

SignalR - WebSocket connection between Client & Server

![BlazorServer.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/BlazorServer.png)

---
## # Blazor Server (2)

When using the **InteractiveServer** render mode with Blazor, the server opens a websocket connection to the client. Any user interaction will be sent to the server via this connection. The server computes the necessary UI changes and sends back to the client what needs to be updated.

- We need the InteractiveServer mode when we have handlers.

The underlying technology used to send data via the websocket connection is **SignalR**.

**SignalR** is a nuget package that can be used separately from Blazor to implement real-time communication between client and server using websockets.

---
## # Blazor

Blazor is a **Framework** for creating interactive user interfaces for web applications.

Blazor can therefore be compared with technologies such as Angular or React.

---

![FeTechnologies.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/FeTechnologies.png)

---
## # Blazor Motivation

When using Blazor …

- both frontend and backend of an application can be implemented using a single **technology** (.NET)
- you can use **.NET languages** (C#, F#) instead of JavaScript

---
## # Razor Pages

Blazor applications are built up using **Razor Pages**.

Razor is a templating engine (similar to PHP) where you can mix static HTML with C# code to implement dynamic views (e.g. render a certain element based on a condition).

---
## # HTML and code

HTML is the structure of your application.

Use C# code to add logic to it.

```csharp
<h1>Hallo @name!</h1>
@code {
	string name = "Hugo";
}
```

---
## # Razor code

To implement **logic** inside your Razor Pages you can use **Razor expressions**.

There are 3 types of Razor expressions:

- Razor expression
- Razor code block
- Razor control structure

Detailed info: [Razor expression docs](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/razor?view=aspnetcore-8.0)

---
## # Razor expressions

Razor expressions will be evaluated to a value upon execution.

There are 2 forms:

- implicit Razor expressions
- explicit Razor expressions

---
## # Implicit Razor expressions

An implicit Razor expression starts with the @ symbol, followed by an expression. The @ symbol tells Razor to start evaluating C# code.

You can use implicit Razor expressions to access values of variables.

```csharp
<span>Zeitpunkt: @DateTime.Now </span>
```

---

```csharp
<span>Hallo @CustomToUpper(Name) </span>

@code {
	public string Name { get; set; } = "Hugo";

	string CustomToUpper(string value) =>
		value.ToUpper();
}
```

---
## # Explicit Razor expressions

An explicit Razor expression starts with the @ symbol, followed by an expression surrounded by parentheses.

You need to use explicit Razor expressions when the parser cannot infer the syntax of the expression correctly.

```csharp
<span>Ergebnis: 2 + 2 = @(2 + 2) </span>
```

---
## # Razor code blocks

A Razor Code Block starts with an @ symbol, followed by program definitions (classes, methods, variables) surrounded by braces.

Whereas Razor expressions will always be evaluated to a value, code blocks aren’t meant to be rendered.

---

```csharp
<span>Hallo @CustomToUpper(Name) </span>

@code {
	public string Name { get; set; } = "Hugo";

	string CustomToUpper(string value) =>
		value.ToUpper();
}
```

---
## # Razor control structures

Razor control structures are an extension of code blocks.

They are meant to control the program execution within Razor Pages.

---
## # Control structure: if

```csharp
@if(names.Length > 0) {
	<span>We have friends: @names.Length</span>
} else {
	<span>Lets buy a dog!</span>
}

@code {
	string[] names = {"Tobi", "Nikolei", "Franz"};
}
```

---
## # Control structure: switch

```csharp
@switch(index) {
	case 200: <p>OK</p> break;
	case 404: <p>Not Found</p> break:
	default:
		<p>code is wrong</p>
		break;
}

@code {
	int code = 404;
}
```

---
## # Control structure: for

```csharp
@for(var i = 0; i < persons.Length; i++) {
	var person = persons[i];
	<p>Name: @person.Name</p>
	<p>Age: @person.Age</p>
}

@code {
	Person[] persons = {
		// ...
	};
}
```

---
## # Control structure: foreach

```csharp
@foreach(var person in persons) {
	<p>Name: @person.Name</p>
	<p>Age: @person.Age</p>
}

@code {
	Person[] persons = {
		// ...
	};
}
```

---
## # Single Page Application

**SPA**s are web applications, that consist of a single HTML page.

Contents of the website are loaded dynamically and on-demand.

---
## # SPA - Communication

Internally, **SPA**s don’t use navigation between websites.

The **presentation flow** is not interrupted, because no complete pages have to be loaded and rendered. Only parts of the application are reloaded as needed.

---
## # WA vs. SPA

document = HTML-Structure
AJAX (Asynchronous JavaScript and XML) = request, that only requests data in JSON format

![WaVsSpa.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/WaVsSpa.png)

---
## # WA vs. SPA (2)

WA - complete site gets replaced
SPA - when data is ready - only a single component gets replaced = presentation flow not interrupted

![WaVsSpa2.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/WaVsSpa2.png)

---
## # SPA Advantages

!Quick Loading Time - not everyhing gets loaded fast
- first access on SPA is slow

![SpaAdvantages.png](https://deep-thought.norwin.at/tech-kb/web-development/assets/SpaAdvantages.png)

---
## # Components

**Components** are the _building blocks_ of an SPA.

At runtime components are being loaded and disposed. The page in which the components are rendered stays the same.

---
## # Razor Components

**Blazor** applications consist of **Razor components** .

Razor components are a mixture of HTML and program code (C#). Razor components are being compiled to C# classes.

---
## # Razor Components - Example

```csharp
<!-- ----------------------------------------------- -->
<!-- Component: Card
<!-- ----------------------------------------------- -->
<div class="card">
    <div class="card-body">
        <h3 class="card-title">@Movie.Title</h3>
        <p class="small">@Movie.Description</p>
    </div>
</div>
@code {
    [Parameter]
    public Movie Movie { get; set; }
}
```

---
## # Using Razor Components

```csharp
<!-- ----------------------------------------------- --> 
<!-- Using the Card component
<!-- ----------------------------------------------- -->
   ...
   <div class="col">
     <div class="caed-deck">
         <Card Movie="@MovieCard"/>
     </div>
   </div>
...
@code{
    Movie MovieCard { get; set; }
}
```

---
## # Component Lifecycle

When a component loads, a couple of methods are being called on the component by the framework.

These methods are called **lifecycle methods**.

Every Razor component inherits from `ComponentBase` in which these lifecycle methods are defined.

---
## # Lifecycle methods

```csharp
<div> ... </div>
@code {
    protected override Task SetParametersAsync(
          ParameterView parameters
    ) {...}
    protected override void OnInitialized() {...}
    protected override async Task OnInitializedAsync() {...}
    protected override void OnParametersSet() {...}
    protected override async Task OnParametersSetAsync() {...}
    protected override void OnAfterRender(
        bool firstRender
    ) {...}
}
```

---
## # Lifecycle methods - Execution order

```csharp
<div> ... </div>
@code {
    (1) protected o. Task SetParametersAsync(
          ParameterView parameters
       ) {...}
    (2) protected o. async Task OnInitialized() {...}
    (3) protected o. async Task OnParametersSet() {...}
    (4) protected o. async Task OnAfterRender(
          bool firstRender
       ) {...}
}
```

---
### # SetParametersAsync

This method is called when the parameters for the component are being loaded.
You can intercept the parameters and do actions based on them.

```csharp
<div> ... </div>
@code {
    protected override Task SetParametersAsync(
       ParameterView parameters
) {...} }
```

---
### # OnInitializedAsync

This method is called so you can initialize the internal fields of the component.
It only runs once!
Use this instead of a constructor.

```csharp
<div> ... </div>
@code {
    protected override Task OnInitialisedAsync() {...}
}
```

---
### # OnParametersSetAsync

This method is called when parameters are changed.
Runs every time the value changes!

```csharp
<div> ... </div>
@code {
    protected override Task OnParametersSetAsync() {...}
}
```

---
### # OnAfterRenderAsync

This method is called, after the component has been rendered on the page.

```csharp
<div> ... </div>
@code {
    protected override Task OnAfterRenderAsync() {...}
}
```

---
## # Navigation

In Blazor there are 2 kinds of components:

- Razor Page
- Razor Component

---
## # Page Directive

**Razor Pages** are components that can be loaded explicitly via a **URL**. Use the `@page` directive to make a page out of a component.

```csharp
@page "/hallo"
<div>...</div>
@code { 
	...
}
```

---
## # Path Variables

Blazor allows passing parameter values as part of the path in the URL. Per default, `string` will be used as the type for parameters.

```csharp
@page "/people/{Message}"
<div>
	...
    <h2>@Message</h2>
</div>
@code {
    [Parameter]
    public string Message { get; set; }
}
```

---
## # Path variables - Data Types

You can explicitly set different data types when you don’t want to use a string for path variables.

```csharp
@page "/people/{Id:int}"
<div>...</div>
@code {
    [Parameter]
    public int Id { get; set; }
}
```

---
## # Path variables - Multiple routes

You can also specify multiple paths for the same component.

```csharp
@page "/people/{Id:int}"
@page "/people/{Message}"
@page "/people"
<div>...</div>
@code {
    [Parameter]
    public int? Id { get; set; }
    [Parameter]
    public string? Message { get; set; }
}
```

---
## # Navigation

To navigate between websites you can use the **anchor element**.

```csharp
@page "/people"
<div>
	...
    <a href="/authors/1" class="...">Autoren</a>
</div>
@code { ...
}
```

---
## # Navigation - Programmatically

To navigate programmatically, you can use the `NavigationManager`.

```csharp
@page "/people"
@inject NavigationManager _navManager
<div>...</div>
@code {
    public void Authorize(){
		...
       _navManager.NavigateTo("/authors/2", true);
    }
}
```

---
## # Navigation - Programmatically (2)

```csharp
@page "/people"
@inject NavigationManager _navManager
<div>
   <button @onclick="@(
        () => _navigationManager.NavigateTo(
            "/authors/2", true
        )
   )">
     Load Author
   </button>
</div>
@code { 
	...
}
```

---
## # Dependency Injection

To inject dependent types into Razor components, use the `inject` directive.

```csharp
@page "/people"
@inject NavigationManager _navManager
```

---
## # Binding

There are 2 directions of data flow when working with values in Blazor:

- Binding the value of a variable to an element on the page, so that when the value of the variable changes, the updated value gets shown on the page.
- Binding the value of an input element on the page to a variable, so that when the user enters a value on the page, the value of the variable gets updated.

---
## # Binding: variable to component

We already know how to achieve this, by simply using the Razor syntax to render the value of a variable.

```csharp
<p>@message</p>
<input type="text" value="@message" />

@code {
	private string message = "Hello";
}
```

Whenever the value of the variable message changes, the changes will be reflected on the rendered page.

---
## # Binding: input element to variable

There are 2 ways of achieving this:

- Change event handlers
- 2-way binding

---
## # Change event handlers

You can register an event handler to listen on changes of an input element.

```csharp
<input type="text" @oninput="OnChange"/>

@code {
    private void OnChange(ChangeEventArgs obj)
    {
        var updatedValue = obj.Value as string;
    }
}
```

---
## # 2-way binding

You can combine the two binding variants to achieve a two-way binding, where an input element updates a variable and the other way round.

```csharp
<input type="text" @oninput="OnChange" value="@message">
</input>

@code {
    private string message = "Hello";

    private void OnChange(ChangeEventArgs obj)
    {
        message = obj.Value as string ?? message;
    }
}
```

---
## # 2-way binding (2)

Instead of wiring up the 2-way binding manually, you can use the predefined `@bind-value` directive, which does exactly that for you - show the value of the variable in HTML and use an event handler to update the variable in your code upon user inputs.

```csharp
<input type="text" @bind-value="message"/>

@code {
    private string message = "Hello";
}
```

---
## # Forms

Forms are used to capture and process data, entered by users.

Using Razor Expressions you can bind objects directly to the values of the form. Use the `bind-value` attribute for that.

---
## # Forms - Class

```csharp
public class Movie {
   [StringLength(100)]
   [Required]
   public string Title { get; set;}
   [StringLength(400)]
   [Required]
   public string Description { get; set; }
   ...
   public int Duration { get; set; }
   ...
   public EGenre Genre { get; set; }
}
```

---
## # Forms - Initialization

```csharp
<EditForm Model="Movie" OnValidSubmit="Create"
    class="form">
    <DataAnnotationsValidator/>
...
</EditForm>
@code{
    public Movie Movie { get; set; } = new ();
    public void Create() {
       ...
	} 
}
```

---
## # Forms - Data Binding

```csharp
<EditForm ... class="form">
    <DataAnnotationsValidator/>
    <div class="form-row">
        <div class="form-group col-md-5">
            <label for="movieTitle" class="small
                text-info">title:</label>
            <InputText class="form-control
                form-control-sm" id="movieTitle"
                placeholder="input title"
                @bind-Value="@Movie.Title"/>
            <ValidationMessage For="()=> Movie.Title"/>
        </div>
</div>
...
</EditForm>
```

---
## # Forms - Send Request

```csharp
<EditForm ... class="form">
    ...
    <div class="form-row mt-3">
       <NavLink href="movies" class="btn btn-danger
           btn-sm">Cancel</NavLink>
       <button class="btn btn-info ml-2
           btn-sm">@ButtonLabel</button>
    </div>
</EditForm>
```

---