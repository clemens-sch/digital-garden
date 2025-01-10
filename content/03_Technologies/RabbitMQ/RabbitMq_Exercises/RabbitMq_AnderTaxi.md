#Technologies 

[Exercise](https://deep-thought.norwin.at/notes/tech-kb/cloud/exercise/exercise---ander-taxi/) 

---
## # Definition

- A passenger may request a taxi ride, sending their current location and their desired destination.
- The system will pick one or more available taxi drivers (prefer drivers that are close to the passenger).
- The taxi driver can accept or decline rides.
- The system makes sure that only one taxi driver is assigned to pick up a passenger.
- A taxi driver is in one of the following states:
    - Unavailable
    - Available
    - OfferedRide
    - OnRouteToPassenger
    - OnRouteToDestination
- A taxi driver will at all times report its current location and state to the system.
- Passengers will receive an invoice upon a completed ride, based on the distance and a rate (€/km).
- The system will log all messages.

---
## # Assignment

- Decide on an architecture.
	- Architecture diagrams created in class are available in the SEW teams channel and may be used for reference.
	- You can still refine your architecture
	- Provide an updated system overview diagram in your upload.

![[RabbitMq_AnderTaxi 2024-12-10 08.08.02.excalidraw]]

Elements:
- User
- Taxi
- ClosestTaxiService
	- finds taxis, that are nearby the passenger's location
- TaxiStateService
	- polling
	- delivers the current state of the taxi
- TaxiManagementService
	- central service for managing taxi-requests, offers and assignments
	- guarantees: one taxi per passenger
- InvoiceService
	- service, that handles the creation of invoices

---

- Implement the messaging infrastructure in RabbitMQ
	- Create and configure all necessary exchanges and queues to ensure message delivery to the individual services in your system.
	- No upload needed, but students may be picked to showcase their configuration.

```cmd
docker run -d --hostname rabbitmq --name rabbit-server -p 8080:15672 -p 5672:5672 rabbitmq:3-management
```

Login
- guest
- guest

### # Code

RabbitMqService.cs

```csharp
using System.Text;  
using RabbitMQ.Client.Events;  
  
namespace AnderTaxi_RabbitMq;  
  
using RabbitMQ.Client;  
  
public class RabbitMqService  
{  
    private readonly string _rabbitMqHost = "localhost";  
    // topic exchange  
    private readonly string _exchangeName = "ride_service_exchange";  
  
    // 4 queues with routing-keys  
    private readonly Dictionary<string, string> _queueBindings = new()  
    {  
        { "log_service_queue", "log.#" },  
        { "invoice_service_queue", "invoice.#" },  
        { "passenger_service_queue", "passenger.#" },  
        { "driver_service_queue", "driver.#" }  
    };  
  
    // creation - topic-exchanges + bind queues with routing-keys  
    public async Task ConfigureMessagingInfrastructure()  
    {  
        ConnectionFactory factory = new()  
        {  
            Uri = new Uri("amqp://guest:guest@localhost:5672"),  
            HostName = _rabbitMqHost  
        };  
  
        await using var cnn = await factory.CreateConnectionAsync();  
        var channel = await cnn.CreateChannelAsync();  
        await channel.ExchangeDeclareAsync(exchange: _exchangeName, type: ExchangeType.Topic);  
  
        // create queues - bind with routing-keys  
        try  
        {  
            foreach (var (queue, routingKey) in _queueBindings)  
            {  
                Console.WriteLine($"Declaring queue: {queue} with routingKey: {routingKey}");  
                await channel.QueueDeclareAsync(queue: queue, durable: false, exclusive: false, autoDelete: false);  
                await channel.QueueBindAsync(queue: queue, exchange: _exchangeName, routingKey: routingKey);  
                Console.WriteLine($"Queue {queue} declared and bound.");  
            }  
        }        catch (Exception ex)  
        {  
            Console.WriteLine($"Error creating or binding queue: {ex.Message}");  
        }  
  
        Console.WriteLine("RabbitMQ infrastructure configured successfully.");  
    }  
  
    public async Task PublishMessage(string routingKey, string message)  
    {  
        ConnectionFactory factory = new()  
        {  
            Uri = new Uri("amqp://guest:guest@localhost:5672"),  
            HostName = _rabbitMqHost  
        };  
  
        await using var cnn = await factory.CreateConnectionAsync();  
        var channel = await cnn.CreateChannelAsync();  
  
        var body = Encoding.UTF8.GetBytes(message);  
  
        await channel.BasicPublishAsync(  
            exchange: _exchangeName,  
            routingKey: routingKey,  
            mandatory: false,  
            body: body  
        );  
  
        Console.WriteLine($"Message published: {message} with routingKey: {routingKey}");  
    }  
  
    public async Task<string> ConsumeMessages(string queueName)  
    {  
        ConnectionFactory factory = new()  
        {  
            Uri = new Uri("amqp://guest:guest@localhost:5672"),  
            HostName = _rabbitMqHost,  
            VirtualHost = "/"  
        };  
  
        await using var cnn = await factory.CreateConnectionAsync();  
        var channel = await cnn.CreateChannelAsync();  
  
        var message = string.Empty;  
        var consumer = new AsyncEventingBasicConsumer(channel);  
        consumer.ReceivedAsync += (sender, args) =>  
        {  
            var body = args.Body.ToArray();  
            message = Encoding.UTF8.GetString(body);  
            Console.WriteLine($"Message received in {queueName}: {message}");  
            return Task.CompletedTask;  
        };  
          
        var consumerTag = await channel.BasicConsumeAsync(queueName, true, consumer);  
  
        await Task.Delay(5000);  
          
        await channel.BasicCancelAsync(consumerTag);  
  
        Console.WriteLine($"Started consuming messages from {queueName}");  
  
        return message;  
    }  
}
```

PublishRequest.cs

```csharp
namespace AnderTaxi_RabbitMq;  
  
public class PublishRequest  
{  
    public string RoutingKey { get; set; }  
    public string Message { get; set; }  
}
```

Program.cs

```csharp
using System.Text;  
using AnderTaxi_RabbitMq;  
using RabbitMQ.Client;  
  
var builder = WebApplication.CreateBuilder(args);  
  
builder.Services.AddSingleton<RabbitMqService>();  
  
var app = builder.Build();  
  
var rabbitMqService = app.Services.GetRequiredService<RabbitMqService>();  
await rabbitMqService.ConfigureMessagingInfrastructure();  
  
app.MapGet("/", () => "RabbitMQ Infrastructure Configured!");  
  
app.MapPost("/publish", async (PublishRequest request) =>  
{  
    var rabbitMqService = app.Services.GetRequiredService<RabbitMqService>();  
    await rabbitMqService.PublishMessage(request.RoutingKey, request.Message);  
    return Results.Ok($"Message published to routing key {request.RoutingKey}: {request.Message}");  
});  
  
app.MapGet("/consume/{queueName}", async (string queueName) =>  
{  
    var rabbitMqService = app.Services.GetRequiredService<RabbitMqService>();  
    var message = await rabbitMqService.ConsumeMessages(queueName);  
    return Results.Ok($"Consuming messages from queue {queueName}: {message}");  
});  
  
app.Run();
```

`/`

![[Pasted image 20250104215133.png]]

---
### # Exchange & Queues

As you can see, the `ride_service_exchange` got created

![[Pasted image 20250104215348.png]]

Also our 4 queues got created.

![[Pasted image 20250104215408.png]]

---
### # Sending Messages

Here we send an invoice.
- The structure is based on the PublishRequest class

Sender:

```json
{
  "routingKey": "invoice.user1",
  "message": "5,99€"
}
```

![[Pasted image 20250104215012.png]]

Receiver:

`/consume/invoice_service_queue`

Here we get the price sent above.

![[Pasted image 20250104215207.png]]

---

- Visualize your RabbitMQ infrastructure
	- Upload a diagram showing all the exchanges, queues and routing configurations in your RabbitMQ message broker.

### # Topicexchange

From publisher to consumer.
- ride_service_exchange
	- log_service_queue
	- passenger_service_queue
	- invoice_service_queue
	- driver_service_queue

![[RabbitMq_AnderTaxi 2025-01-04 21.54.34.excalidraw]]
