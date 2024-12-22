#Technologies 

[Tutorial](https://medium.com/c-sharp-programming/integrating-azure-blob-storage-with-net-b4fc16dfde73)

---
## # Exercise

The Azure storage account can be used for several different things, mainly (but not only): 

- Storing files (pictures, videos, documents, …) 
- Storing data in a table structure 
- Sending and receiving messages in a storage queue. 

Try to find out how to access the storage account from a C# application. 

A simple console application is sufficient. 

The application should be able to do the following. 

1. Take a local textfile and upload it to the storage account into a container called “pupils”.  
    The text file should be named after you (Firstname_Lastname.txt) and contain a short message like “all your base are belong to us”. 
2. Download the previously uploaded textfile and save it under as Firstname_Lastname_Timestamp.txt on your local hard drive. 
3. Make an entry into a table called “pupiltable”. You should have a column for each of these fields: Firstname, Lastname, DateOfBirth.  
    As PartitionKey use “pupils”, as RowKey use any unique ID that identifies you. 
4. Read data from the table. List all entries (the entry made by you and by others in class) and output it in the console. 
5. Write a message in a storage queue called “greetings”. 
6. Write a message consumer that listens for messages being added and displays the content of the message in the console. 

Use information online to find out how to access the service. (e.g. which NuGet package to use). 

You will get a connection string from your teacher to be able to connect to the storage account.

---
## # Assignment

1. NuGets Download:

```
dotnet add package Azure.Storage.Blobs
dotnet add package Azure.Storage.Queues
dotnet add package Azure.Storage.Tables
```

2. Connection String finden

- Take a local textfile and upload it to the storage account into a container called “pupils”.  
    The text file should be named after you (Firstname_Lastname.txt) and contain a short message like “all your base are belong to us”

```csharp
class Program  
{  
    private const string StorageConnectionString = <CONNECTION-STRING>
    private const string ContainerName = "pupils";  
    private const string TableName = "pupiltable";  
    private const string QueueName = "greetings";  
  
    static async Task Main(string[] args)  
    {  
        var firstName = "Clemens";  
        var lastName = "Schmid";  
        var message = "The LFC is going to win the Champions League!";  
        var localFileName = "Clemens_Schmid.txt";  
        var localFilePath = Path.Combine(Environment.CurrentDirectory, localFileName);

		await File.WriteAllTextAsync(localFilePath, message);  
		Console.WriteLine($"File created: {localFilePath}");  
  
		await UploadFileToBlob(localFilePath);
    }

	private static async Task UploadFileToBlob(string filePath)  
	{  
	    var blobServiceClient = new BlobServiceClient(StorageConnectionString);  
	    var containerClient = blobServiceClient.GetBlobContainerClient(ContainerName);  
	    await containerClient.CreateIfNotExistsAsync();  
	  
	    var blobName = Path.GetFileName(filePath);  
	    var blobClient = containerClient.GetBlobClient(blobName);  
	  
	    await blobClient.UploadAsync(filePath, true);  
	    Console.WriteLine($"Uploaded {blobName} to blob storage.");  
	}
}
```

![[Pasted image 20241213125257.png]]


- Download the previously uploaded textfile and save it under as Firstname_Lastname_Timestamp.txt on your local hard drive. 

```csharp
...
private static async Task UploadFileToBlob(string filePath)  
{  
    var blobServiceClient = new BlobServiceClient(StorageConnectionString);  
    var containerClient = blobServiceClient.GetBlobContainerClient(ContainerName);  
    await containerClient.CreateIfNotExistsAsync();  
  
    var blobName = Path.GetFileName(filePath);  
    var blobClient = containerClient.GetBlobClient(blobName);  
  
    await blobClient.UploadAsync(filePath, true);  
    Console.WriteLine($"Uploaded {blobName} to blob storage.");  
}
```

![[Pasted image 20241213125235.png]]


- Make an entry into a table called “pupiltable”. You should have a column for each of these fields: Firstname, Lastname, DateOfBirth.  
    As PartitionKey use “pupils”, as RowKey use any unique ID that identifies you. 

```csharp
...
private static async Task InsertIntoTable(DateTime dateOfBirth)  
{  
    var tableServiceClient = new TableServiceClient(StorageConnectionString);  
    var tableClient = tableServiceClient.GetTableClient(TableName);  
    await tableClient.CreateIfNotExistsAsync();  
  
    var rowKey = Guid.NewGuid().ToString();  
    var entity = new TableEntity("pupils", rowKey)  
    {  
        { "DateOfBirth", dateOfBirth.ToString("yyyy-MM-dd") }  
    };  
  
    await tableClient.AddEntityAsync(entity);  
    Console.WriteLine("Inserted Clemens Schmid into table storage.");  
}
```

![[Pasted image 20241213130031.png]]


-  Read data from the table. List all entries (the entry made by you and by others in class) and output it in the console. 

