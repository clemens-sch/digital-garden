#Software-Engineering 

[YT-Video](https://www.youtube.com/watch?v=Ijw0Q5EOqmE)

---
## # AutoMapper ? 

Is a library for mapping objects from one type to another.

When working with sensitive data you will probably gonna use DTOs [[SE_DTO]].
But you have to map these DTOs in order to not use your standard class.

---
## # Without AutoMapper

Let's assume we have an API or some HTTP-Methods:

```csharp
[HttpGet]
public IActionResult GetInvoice()
{
	Invoice invoiceFromDb = new Invoice()
	{
		Id = 1,
		Description = "Grocery",
		Amount = 100
	};

	// This is, what we wanna get rid of & we can
	InvoiceDto invoiceDto = new InvoiceDto()
	{
		Description = invoiceFromDb.Description,
		Amount = invoiceFromDb.Amount	
	};

	return Ok(invoiceDto);
}
```

Instead, do the following:

```csharp
// make sure to inject the IMapper as "mapper"

[HttpGet]
public IActionResult GetInvoice()
{
	Invoice invoiceFromDb = new Invoice()
	{
		Id = 1,
		Description = "Grocery",
		Amount = 100
	};

	// Hey Mapper, please map the invoiceFromDb as an InvoiceDto, thanks
	InvoiceDto invoiceDto = mapper.Map<InvoiceDto>(invoiceFromDb);

	return Ok(invoiceDto);
}
```

---
### # AutoMapperProfile

Create a Profile for our AutoMapper.

```csharp
public class AutoMapperProfile : Profile  
{  
    public AutoMapperProfile()  
    {
	    // Map Invoice class into DTO class
	    CreateMap<Invoice, InvoiceDto>();
	}
}
```

Also make sure to register the AutoMapper Profile as Service.

```csharp
...
// link assembly to the AutoMapper Profile
builder.Services.AddAutoMapper(typeof(AutoMapperProfile));
```

---
### # Use Mapper in Controller

Now we can use our `IMapper` because we registered it in DI-Container.

```csharp
public abstract class GenericController<TEntity, TDto, TId>(  
    IRepositoryBase<TEntity, TId> repository,  
    IMapper mapper)  
    : ControllerBase where TEntity : class where TDto : class
{
	protected readonly IRepositoryBase<TEntity, TId> Repository =  
    repository ?? throw new ArgumentNullException(nameof(repository));  
  
	private readonly IMapper _mapper = mapper ?? throw new ArgumentNullException(nameof(mapper));
	...    
}
```

