#Software-Engineering 

[Video](https://www.youtube.com/watch?v=ZDjhuFnZrDU)

---
## # Web State-Management

The web is stateless. That means, that the server fulfills a request & probably forgets about it. 
So the value will be lost.

---
## # Ways to store State

1. in memory state
2. in URLs - querystring parameters
3. state container (dependency injection)
4. client-side storage (localStorage, sessionStorage)
5. server-side storage (database) 

---
## # 1. in Memory State

with usage of attribute `CascadingParameter`

```csharp
//AppState.cs
public class AppState
{
	public int Counter { get; set; }
}
```

```csharp
// MainLayout.razor
<div class="page">
	...
	<main>
		...
		<article ...>
			<CascadingValue Value="@ApplicationState">
				@Body
			</CascadingValue>
		</article>
	</main>
</div>
```

```csharp
// counter.razor
...
<button @onclick="Increment">Click me</button>

@code{
// Value will be set by the parent-component
	[CascadingParameter]
	public AppState ApplicationState { get; set; }

	private void Increment()
	{
		ApplicationState.Counter++;
	}
}
```

---
## # 2. in URL

```csharp
// counterParameter.razor
<p role="status">Current count: @CurrentCount</p>  
  
<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>  
  
@code {  
    [Parameter]  
    public int CurrentCount { get; set; }
    
    [Parameter]  
    public EventCallback<int> CounterChanged { get; set; }  
  
    private async Task IncrementCount()  
    {  
        CurrentCount++;  
        await CounterChanged.InvokeAsync(CurrentCount);  
    }  
}
```

```csharp
// counterUrl.razor
@page "/counter-url"  
@page "/counter-url/{Counter:int}"  
@using BlazorStateManagement.Components  
@inject NavigationManager NavigationManager  
  
<PageTitle>Counter</PageTitle>  
  
<CounterParameter CurrentCount="Counter" CounterChanged="Callback"/>  
  
@code {   
	[Parameter]  
    public int Counter { get; set; }  
  
    private void Callback(int counter)  
    {  
        Counter = counter;  
        NavigationManager.NavigateTo($"/counter-url/{Counter}");  
    }  
}
```

---
## # 3. Dependency Injection

The counter's value will be shared across the application using DI.

```csharp
// DAppState.cs
public class DAppState
{
	public int Counter { get; set; }
}
```

```csharp
// Program.cs
...
builder.Services.AddSingleton<DAppState>();
```

```csharp
// counter.razor
@inject DAppState DAppState

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>  

@code{
	private void IncrementCount()  
	{  
		DAppState.Counter++;  
	}
}
```

---
## # 4. Browser Storage - Client-Side

Using Browser Storage system in Blazor. Information gets saved in cache.

```csharp
// Program.cs
...
builder.Services.AddBlazoredLocalStorage();
```

```csharp
// counterLocalStorage.razor
@page "/counter-localstorage"  
@using Blazored.LocalStorage  
@using BlazorStateManagement.Components  

@inject ILocalStorageService LocalStorage  

<PageTitle>Counter</PageTitle>  
  
<CounterParameter CurrentCount="counter" CounterChanged="OnCounterChanged"/>  
  
@code {  
  
    private int counter = 0;  
  
    protected override async Task OnInitializedAsync()  
    {  
        counter = await LocalStorage.GetItemAsync<int>("counter");  
    }  
  
    private async Task OnCounterChanged(int cnt)  
    {  
        await LocalStorage.SetItemAsync("counter", cnt);  
        counter = cnt;  
    }  
  
}
```
