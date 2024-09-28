#Technologies 

---

![TodosAppAdvanced.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/TodosAppAdvanced.excalidraw.svg)

Extend your application from [Docker Exercise 7](https://deep-thought.norwin.at/tech-kb/containers/exercises/Docker-Exercise-7/). Add a new functionality that allows the generation of PDF documents, so you can print your TODO-Lists.

Add a new button to your web application that starts the PDF generation for your Todo List. When pressing the button, send a message to a container that runs a message-oriented middleware. I recommend using a RabbitMQ image from Docker Hub.

Add another container, with a simple application that you build yourself using C#. The container should connect to the message-oriented middleware and receive messages. The message contains all todo items, that should be shown on the PDF. Save the generated file to a path inside the container and add a bind mount to your host machine, so you can access the generated PDFs.

For generating the PDFs you can use any NuGet package that you like, e.g. QuestPDF. To use the free Community License for QuestPdf you have to activate it once in code:

```csharp
QuestPDF.Settings.License = LicenseType.Community;
```