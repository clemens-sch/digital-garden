#Definitions 

---
## Serverless Computing

- is a cloud computing execution model
- Cloud Provider allocates machine resources on demand
- Developer takes only care of code that is being deployed
- Server management is hidden from developer - hence the name “Serverless”
- Of course servers are still needed to run the code (name might be misleading)

---

## Azure Functions

- the Azure way of doing serverless computing
- Developers deploy a single or multiple **functions** grouped in a **function app**.

---

## Example

```csharp
[Function("RecurringEvent")]
public static string Run(  
	[TimerTrigger("*/10 * * * * *")] MyInfo myTimer,
	FunctionContext context)
	{  
		var logger = context.GetLogger("RecurringEvent");  
		logger.LogInformation($"Function executed at: {DateTime.Now}");
	}
```

---

## Structure

- an Azure Function is a method/function implemented in a programming language
- every Function needs a trigger/input binding (when will the function be executed)
- a function might have an output binding

---

## Supported platforms

- Azure function implementation are limited to these (currently) available platforms:
    - .NET
    - Node.js
    - Python
    - Java
    - PowerShell Core
    - Custom Handlers (any webserver)

---

## Triggers / Input Bindings

- Azure functions support these types of triggers / input bindings:
    - Timer Trigger
    - HTTP Trigger
    - Queue Trigger
    - ServiceBus Queue Trigger
    - ServiceBus Topic Trigger
    - Blob Trigger
    - Cosmos DB Trigger
    - Event Grid Trigger
    - Event Hub Trigger
    - Custom Bindings (written in .NET)

---

## Function outputs

- Nearly every Azure function will have some sort of output - JSON returned from a HTTP trigger, messages being saved in a queue, files being uploaded to the blob storage and so on.
- You can always use the according NuGet packages to manually handle the outputs.
- Often times this is needed when we need fine-granular control.

```
[FunctionName("upload")]    public static async Task<IActionResult> RunAsync(        [HttpTrigger(AuthorizationLevel.Function,          "post", Route = "upload")] HttpRequest req,        ILogger log){    // read some data    // ...
    blobClient.Upload(stream);    return new OkResult();}
```

---

## Output bindings

There also exist output bindings which simplify handling output values. Eg

```
[FunctionName("ResizeImage")]public static void Run(  [BlobTrigger("sample-images/{name}")] Stream image,  [Blob("sample-images-sm/{name}", FileAccess.Write)]    Stream imageSmall){  IImageFormat format;
  using (Image<Rgba32> input =    Image.Load<Rgba32>(image, out format))  {    ResizeImage(input, imageSmall, format);  }}
public static void ResizeImage(  Image<Rgba32> input, Stream output, IImageFormat format){  input.Mutate(x => x.Resize(640, 400));  input.Save(output, format);}
```

---

## Output bindings (2)

```
[StorageAccount("MyStorageConnectionAppSetting")]public static class QueueFunctions{  [FunctionName("QueueOutput")]  [return: Queue("myqueue-items")]  public static string QueueOutput(    [HttpTrigger] dynamic input,  ILogger log)  {    return input.Text;  }}
```

---

## Azure functions documentation

All important information on the development of Azure functions can be found at [Azure Functions Documentation](https://learn.microsoft.com/en-us/azure/azure-functions/)

Look for the section “Reference / Triggers and bindings” on detailed information for the individual bindings, e.g. [Blob Storage Output Binding](https://learn.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob-output?tabs=python-v2%2Cin-process%2Cnodejs-v4&pivots=programming-language-csharp)

---

## Configuration values

When developing locally, you can store any configuration values in local.settings.json

When dealing with Azure functions we often deal with accessing storage accounts or other Azure services.

You can store your connection strings inside the local.settings.json file. This file is only for your local development and won’t be deployed to Azure.

```
{  "IsEncrypted": false,  "Values": {  "AzureWebJobsStorage": "UseDevelopmentStorage=true",  "FUNCTIONS_WORKER_RUNTIME": "dotnet",  "StorageConnectionString": "UseDevelopmentStorage=true"  }}
```

---

## Reading configuration values

Often you can access configuration values just by referencing the names in the attributes:

```
[StorageAccount("StorageConnectionString")]public static class QueueFunctions{  // ...}
```

This example will use the connection string named `StorageConnectionString` from the `Values`-Section of the local.settings.json file for all input and output bindings of all functions in that class.

---

## Reading configuration values (2)

When manually connecting and accessing to other services, or when we need to read any other configuration value we can do this by using the `ConfigurationBuilder`:

```
var config = new ConfigurationBuilder()    .SetBasePath(context.FunctionAppDirectory)    .AddJsonFile("local.settings.json",      optional: true, reloadOnChange: true)    .AddEnvironmentVariables()    .Build();
var blobServiceClient =  new BlobServiceClient(config["StorageConnectionString"]);
```

---

## Hosting

- Consumption (Serverless)
    - Optimized for serverless and event-driven workloads
- Functions Premium
    - Event based scaling and network isolation, ideal for workloads running continuously
- App Service Plan
    - Fully isolated and dedicated environment suitable for workloads that need large SKUs or need to co-locate Web Apps and Functions.

We will only focus on Consumption plans (serverless)

---

## Execution Time Limitations

The execution of a single Azure Function call is limited with different time frames per hosting option. The timeouts can be configured in the host.json file.

|Plan|Default|Maximum|
|---|---|---|
|Consumption|5 min|10 min|
|Premium|30 min|Unlimited*|
|Dedicated|30 min|Unlimited*|

* Guaranteed for up to 60 minutes. OS and runtime patching, vulnerability patching, and scale in behaviors can still cancel function executions so ensure to write robust functions.

---

## Azure functions in .NET

- There are 2 execution models:
    - Worker process
        - function shares the same execution environment as host
        - every function in a function app needs to target same .NET version
        - only LTS versions supported
        - will be deprecated in the future
    - Isolated
        - functions run in an isolated process
        - functions can use different .NET versions in a flexible way
        - supports latest .NET versions
        - recommended for future use
        - some of the bindings are not fully supported yet

---

## Azure functions roadmap

![dotnet-functions-roadmap.png](https://deep-thought.norwin.at/_astro/dotnet-functions-roadmap.DrJZruHR_1gOpr0.webp)

---

## Tools for Function development

- Rider
    - Azure Toolkit for Rider (Install under Settings/Plugins)
- VS Code
    - Azure Functions Extension (comes as part of Azure Tools Extension)
- [Azure Storage Explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer)
- Local storage simulator [Azurite](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite?tabs=visual-studio)