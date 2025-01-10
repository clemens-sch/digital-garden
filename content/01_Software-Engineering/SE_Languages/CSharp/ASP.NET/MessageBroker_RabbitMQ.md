#Software-Engineering 

---

Docker Container:

```cmd
docker run -d --hostname rabbitmq --name rabbit-server -p 8080:15672 -p 5672:5672 rabbitmq:3-management
```

---
## # Sender & Receiver

When working with Message Queues (RabbitMQ) there is a Sender and a Receiver.

Sender
- "producer"
- sends message into exchange>queue

Receiver
- "consumer"
- take the message(s) from the queue and consume it

---
## # Create Sender

Exchange, Queues, Messages.
Here we create a **Sender**, that allows connecting to the queue and sending messages to the queue.

```csharp
// Webserver :15672; Sender :5672  
// Connection String  
ConnectionFactory factory = new()  
{  
    Uri = new Uri("amqp://guest:guest@localhost:5672"),  
    ClientProvidedName = "Rabbit App"  
};  
  
await using var cnn = await factory.CreateConnectionAsync();  
var channel = await cnn.CreateChannelAsync();  
var exchangeName = "DemoExchange";  
var routingKey = "demo-routing-key";  
var queueName = "DemoQueue";  
  
await channel.ExchangeDeclareAsync(exchangeName, ExchangeType.Direct);  
await channel.QueueDeclareAsync(queueName, false, false, false, null);  
await channel.QueueBindAsync(queueName, exchangeName, routingKey, null);  
  
var messageBodyBytes = Encoding.UTF8.GetBytes("Hello World!");  
await channel.BasicPublishAsync(  
    exchange: exchangeName,  
    routingKey: routingKey,  
    mandatory: false,  
    body: messageBodyBytes  
);  
  
await channel.CloseAsync();  
await cnn.CloseAsync();
```

Nun haben wir einen Demo-Exchange mit einer DemoQueue angelegt:

![[Pasted image 20250104191449.png]]

In der Queue befindet sich bereits eine Message:

![[Pasted image 20250104191515.png]]

Nun haben wir den Sender konfiguriert, allerdings fehlt uns noch der Receiver, der die Message ausliest und verarbeitet.

---
## # Create Receiver

```csharp
ConnectionFactory factory = new()  
{  
    Uri = new Uri("amqp://guest:guest@localhost:5672"),  
    ClientProvidedName = "Rabbit Receiver1 App"  
};  
  
await using var cnn = await factory.CreateConnectionAsync();  
var channel = await cnn.CreateChannelAsync();  
var exchangeName = "DemoExchange";  
var routingKey = "demo-routing-key";  
var queueName = "DemoQueue";  
  
await channel.ExchangeDeclareAsync(exchangeName, ExchangeType.Direct);  
await channel.QueueDeclareAsync(queueName, false, false, false, null);  
await channel.QueueBindAsync(queueName, exchangeName, routingKey, null);  
  
// prefetchSize: 0 - we don't care how big the message is  
// prefetchCount: 1 - process one message at a time  
// global: false - we apply this only for this instance  
await channel.BasicQosAsync(0, 1, false);  
  
// consuming the message - decode it as a string  
var consumer = new AsyncEventingBasicConsumer(channel);  
consumer.ReceivedAsync += (sender, args) =>  
{  
    var body = args.Body.ToArray();  
    var message = Encoding.UTF8.GetString(body);  
    Console.WriteLine($"Message Received: {message}");  
    // Acknowledgement, that this message was delivered successfully  
    channel.BasicAckAsync(args.DeliveryTag, false);  
    return Task.CompletedTask;  
};  
  
var consumerTag = await channel.BasicConsumeAsync(queueName, false, consumer);  
// cancel the consumer  
await channel.BasicCancelAsync(consumerTag);  
  
await channel.CloseAsync();  
await cnn.CloseAsync();
```

Message is consumed:

![[Pasted image 20250104192835.png]]

![[Pasted image 20250104192943.png]]
