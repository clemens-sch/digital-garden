#Technologies 

---

![[todosappadvancedexcalidraw.BvKDHmHX_1K1jB9.svg]]

Extend your application from [[Docker-Exercise7_WebApp+Db]]. Add a new functionality that allows the generation of PDF documents, so you can print your TODO-Lists.

Add a new button to your web application that starts the PDF generation for your Todo List. When pressing the button, send a message to a container that runs a message-oriented middleware. I recommend using a RabbitMQ image from Docker Hub.

Add another container, with a simple application that you build yourself using C#. The container should connect to the message-oriented middleware and receive messages. The message contains all todo items, that should be shown on the PDF. Save the generated file to a path inside the container and add a bind mount to your host machine, so you can access the generated PDFs.

For generating the PDFs you can use any NuGet package that you like, e.g. QuestPDF. To use the free Community License for QuestPdf you have to activate it once in code:

```csharp
QuestPDF.Settings.License = LicenseType.Community;
```

---
## # Assignment

docker-compose.yaml:

```yaml
version: "3.8"
services:
  web:
    build:
      context: ./ToDoApp/
      dockerfile: ./ToDoApp/Dockerfile
    environment:
      - ConnectionStrings__DefaultConnection=Server=db;Database=${MYSQL_DATABASE};User=${MYSQL_USER};Password=${MYSQL_PASSWORD};
    ports:
      - "6500:8080"
      - "6501:8081"
    depends_on:
      - db
      - rabbitmq
  db:
    image: mysql:8.0.21
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    volumes:
      - db-data:/var/lib/mysql
  pdf-generator:
    build:
      context: ./ToDoApp/
      dockerfile: ./PdfGenerator_Service/Dockerfile
    volumes:
      - ./pdf-export:/app/pdf
    depends_on:
      - rabbitmq
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"
volumes:
  db-data:

```

PdfGen>Program.cs

```csharp
using QuestPDF.Fluent;  
using QuestPDF.Helpers;  
using RabbitMQ.Client;  
using RabbitMQ.Client.Events;  
using System.Text;  
using Newtonsoft.Json;  
using QuestPDF.Infrastructure;  
using Model.Entities;  
  
// Set QuestPDF license  
QuestPDF.Settings.License = LicenseType.Community;  
  
var tries = 0;  
// Retry connection to RabbitMQ  
while (true)  
{  
    try  
    {  
        tries++;  
        Console.WriteLine("RabbitMQ is online!");  
        break; // Stop retrying once connected  
    }  
    catch  
    {  
        Console.WriteLine($"RabbitMQ not ready yet. Retrying... ({tries})");  
        await Task.Delay(5000); // Wait before retrying  
    }  
}  
  
// Connect to RabbitMQ and declare queue  
var factory = new ConnectionFactory() { HostName = "rabbitmq" };  
using var connection = factory.CreateConnection();  
using var channel = connection.CreateModel();  
  
// Declare a queue for PDF generation requests  
channel.QueueDeclare(queue: "pdf_generation", durable: false, exclusive: false, autoDelete: false, arguments: null);  
  
// Set up consumer to listen for messages  
var consumer = new EventingBasicConsumer(channel);  
consumer.Received += (model, ea) => ReceiveMessage(model, ea);  
  
// Start consuming messages  
channel.BasicConsume(queue: "pdf_generation", autoAck: true, consumer: consumer);  

Console.WriteLine("Waiting for messages...");  
while (true)  
{  
    await Task.Delay(10000); // Keep the process alive  
}  
  
// Handle received messages  
void ReceiveMessage(object model, BasicDeliverEventArgs e)  
{  
    Console.WriteLine("Message received!");  
    var body = e.Body.ToArray();  
    var message = Encoding.UTF8.GetString(body);  
    var todoItems = JsonConvert.DeserializeObject<List<ToDoItem>>(message);  
  
    // Log received items  
    Console.WriteLine("Items received:");  
    foreach (var item in todoItems)  
    {  
        Console.WriteLine($"--Title: {item.Title}, IsDone: {item.IsDone}");  
    }  
    // Generate the PDF  
    GeneratePDF(todoItems);  
}  
  
// Generate PDF for the to-do list  
void GeneratePDF(List<ToDoItem> items)  
{  
    Console.WriteLine("Generating PDF...");  
    var filePath = $"/app/pdf/export{DateTime.Now:yyyyMMdd_hhmmss}.pdf";  
  
    try  
    {  
        // Create the PDF document  
        var document = Document.Create(document =>  
        {  
            document.Page(page =>  
            {  
                page.Margin(20);  
                page.Size(PageSizes.A4);  
                page.DefaultTextStyle(x => x.FontSize(12));  
  
                // Header  
                page.Header()  
                    .Text("To-Do List")  
                    .FontSize(18)  
                    .Bold()  
                    .AlignCenter();  
  
                // Content: List of To-Do items  
                page.Content()  
                    .Column(column =>  
                    {  
                        column.Spacing(5);  
                        column.Item().Text("To-Do Items:");  
                        foreach (var item in items)  
                        {  
                            column.Item().Text($"- {item.Title} (Done: {(item.IsDone ? "Yes" : "No")})");  
                        }  
                    });  
  
                // Footer: Date generated  
                page.Footer()  
                    .AlignCenter()  
                    .Text($"Generated on {DateTime.Now:yyyy-MM-dd HH:mm}");  
            });  
        });  
  
        // Save the PDF  
        document.GeneratePdf(filePath);  
    }  
    catch (Exception e)  
    {  
        Console.WriteLine(e.Message);  
        Console.WriteLine("Error generating PDF");  
    }  
  
    Console.WriteLine($"PDF generated at {filePath}");  
}
```

UI>Home.razor:

```csharp
@page "/"
@using Model.Entities
@using RabbitMQ.Client
@using Newtonsoft.Json
@using System.Text
@inject TodoContext context
@inject HttpClient httpClient
@rendermode InteractiveServer

<PageTitle>ToDoList</PageTitle>

<h1>ToDo List</h1>

<!-- Button to delete all items -->
<button @onclick=DeleteAllItems>Delete Everything</button>

<!-- Input field for adding new items -->
<input type="text" @bind="newItemTitle"/>
<button @onclick="AddItem">Add</button>

<br>
<br>

<!-- Table displaying ToDo items -->
<table>
    <thead>
    <tr>
        <th>Id</th>
        <th>Title</th>
        <th>Is Done</th>
        <th>Action</th>
    </tr>
    </thead>
    <tbody>
    @foreach (var item in context.ToDoItems)
    {
        <tr>
            <td>@item.Id</td>
            <td>@item.Title</td>
            <td>
                <input type="checkbox" @bind="item.IsDone">
            </td>
            <td>
                <!-- Button to delete individual item -->
                <button @onclick="() => DeleteItem(item.Id)">Delete</button>
            </td>
        </tr>
    }
    </tbody>
</table>

<br>
<br>

<!-- Button to save changes to the database -->
<button @onclick="() => context.SaveChanges()">Save Changes</button>

@if (ActivatePDFExport)
{
    <!-- Button to export data to PDF if RabbitMQ is online -->
    <button @onclick="ExportToPdf">Export to PDF</button>
}
else
{
    <p>Export to PDF is disabled until RabbitMQ is online.</p>
}

@code {
    private string newItemTitle { get; set; }  // New item title
    private bool ActivatePDFExport { get; set; }  // Flag to check if PDF export is available

    // Check if RabbitMQ is online
    protected override async Task OnParametersSetAsync()
    {
        ActivatePDFExport = await RabbitMqAlive();
    }

    // Add a new item to the list
    private void AddItem()
    {
        if (string.IsNullOrWhiteSpace(newItemTitle)) return;
        var newItem = new ToDoItem { Title = newItemTitle, IsDone = false };
        context.ToDoItems.Add(newItem);
        context.SaveChanges();
        newItemTitle = string.Empty;  // Clear the input field
    }

    // Delete a single item by id
    private void DeleteItem(int id)
    {
        var item = context.ToDoItems.FirstOrDefault(i => i.Id == id);
        if (item == null) return;
        context.ToDoItems.Remove(item);
        context.SaveChanges();
    }

    // Delete all items
    private void DeleteAllItems()
    {
        context.ToDoItems.RemoveRange(context.ToDoItems);
        context.SaveChanges();
    }
    
    // Check if RabbitMQ is available
    private async Task<bool> RabbitMqAlive()
    {
        try
        {
            var response = await httpClient.GetAsync("http://rabbitmq:15672/api/health/checks/alarms");
            return true;
        }
        catch { return false; }
    }

    // Export data to PDF
    private async Task ExportToPdf()
    {
        if (!await RabbitMqAlive()) return;  // Ensure RabbitMQ is available
        var todoItems = context.ToDoItems.ToList();  // Get all to-do items
        SendToRabbitMq(todoItems);  // Send data to RabbitMQ for PDF generation
    }
    
    // Send data to RabbitMQ for PDF generation
    private static async Task SendToRabbitMq(List<ToDoItem> todoItems)
    {
        var factory = new ConnectionFactory() { HostName = "rabbitmq" };
        using var connection = factory.CreateConnection();
        using var channel = connection.CreateModel();

        // Declare the queue
        channel.QueueDeclare(queue: "pdf_generation", durable: false, exclusive: false, autoDelete: false, arguments: null);

        // Serialize the items and send to the queue
        var message = JsonConvert.SerializeObject(todoItems);
        var body = Encoding.UTF8.GetBytes(message);

        channel.BasicPublish(exchange: "", routingKey: "pdf_generation", basicProperties: null, body: body);
    }
}
```

RabbitMq UI:

![[Pasted image 20241205203341.png]]

Export to .pdf:

![[Pasted image 20241205203350.png]]
