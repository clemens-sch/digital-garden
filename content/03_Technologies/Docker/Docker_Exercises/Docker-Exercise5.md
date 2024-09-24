#Technologies 

---

Create a new C# Minimal API project (use at least dotnet 8) and use this Program.cs file:

```csharp
using Microsoft.AspNetCore.Mvc;

// Setup
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

app.UseSwagger();
app.UseSwaggerUI();

// Endpoint mapping
app.MapPost("/add", SaveFile);

app.MapGet("get/{name}", GetFileContent);

// Endpoint implementation
async Task<IResult> GetFileContent([FromRoute] string name)
{
    var fileNames = FileNames.FromInput(name);
    if (!File.Exists(fileNames.PersistentFilePath))
    {
        return Results.NotFound();
    }

    var content = await File.ReadAllTextAsync(fileNames.PersistentFilePath);
    return Results.Ok(content);
}

async Task SaveFile([FromBody] AddRequest addRequest)
{
    var fileNames = FileNames.FromInput(addRequest.Name);
    await using (var sw = File.CreateText(fileNames.TempFilePath))
    {
        await sw.WriteAsync(addRequest.Content);
    }

    if (File.Exists(fileNames.TempFilePath))
    {
        File.Move(
            fileNames.TempFilePath, 
            fileNames.PersistentFilePath, 
            overwrite: true);
    }
}

if (!Directory.Exists("./temp"))
{
    Directory.CreateDirectory("./temp");
}

if (!Directory.Exists("./persistent"))
{
    Directory.CreateDirectory("./persistent");
}

app.Run();

public class FileNames
{
    public string FileName { get; set; }
    
    public string TempFilePath
    {
        get => "./temp/" + FileName;
    }

    public string PersistentFilePath
    {
        get => "./persistent/" + FileName;
    }

    public static FileNames FromInput(string name) =>
        new()
        {
            FileName = SanitizeFileName(name) + ".txt"
        };

    private static string SanitizeFileName(string name)
    {
        string invalidChars = System.Text.RegularExpressions.Regex.Escape( new string( Path.GetInvalidFileNameChars() ) );
        string invalidRegStr = string.Format( @"([{0}]*\.+$)|([{0}]+)", invalidChars );

        return System.Text.RegularExpressions.Regex.Replace( name, invalidRegStr, "_" );
    }
}

public class AddRequest
{
    public string Name { get; set; }
    public string Content { get; set; }
}
```

In this exercise you will have to perform a lot of steps without any explanation. Your goal is to find your own conclusions on how data is stored inside and outside containers and how volumes work. It is highly recommended that you document your findings after each step.

---
## # Instructions

- Dockerize this application using a `Dockerfile`. You will need these commands:
	- can also be done in Rider when creating a new project + add .dockerfile

    - `dotnet restore <path to .csproj file>`
    - `dotnet build <path to .csproj file> -c Release -o <output path>`
    - `dotnet publish <path to .csproj file> -c Release -o <output path> /p:UseAppHost=false`

---

- Create an image using the tag `volumes-exercise`

```shell
# find dockerfile in the solution
docker build -t volumes-exercise -f Docker_Ex5/Dockerfile .
```

In my case it gets cached, because I built it a second time for a screenshot.

![[Pasted image 20240919132638.png]]

---

- Start a container using this image using `docker run -d -p 8080:8080 --name volumescontainer volumes-exercise`

Now the container is running on port 8080 and forwards also to port 8080.

![[Pasted image 20240919132838.png]]

![[Pasted image 20240919133023.png]]

---

- Try using the provided API to generate and access files within the container

Now open your browser and visit your [loopback-address:8080 + swagger](http://localhost:8080/swagger/index.html).
Here the minimal API should work properly.

Post-Endpoint:

![[Pasted image 20240919133441.png]]

Get-Endpoint:

![[Pasted image 20240919133521.png]]

---

- Use `docker exec -it volumescontainer bash` to connect to a bash shell inside the container. Analyse which files are generated.

Two folders are generated.
- **/temp** - saves files temporarely using the File.CreateText(fileNames.TempFilePath) create method
- **/persistent** - here the posted file is stored & generated
	- "zwettl.txt"

![[Pasted image 20240919135230.png]]

![[Pasted image 20240919135507.png]]

![[Pasted image 20240919135724.png]]

---

- Stop and restart the container using `docker stop volumescontainer` and `docker start volumescontainer`

Stopping container:

![[Pasted image 20240919140017.png]]

Starting container:

![[Pasted image 20240919140055.png]]

---

- Analyse if the files are still there.

The 2 folder & files are still there.

**/temp**:

![[Pasted image 20240919140258.png]]

**/persistent**

![[Pasted image 20240919140337.png]]

---

- Stop and remove the container.

Stopping & removing container:

![[Pasted image 20240919140625.png]]

---

- Start a new container from the image using `docker run -d -p 8080:8080 --rm --name volumescontainer volumes-exercise`

Here we make sure, that our container gets removed ('--rm'-flag) after it is stopped.

![[Pasted image 20240919140827.png]]

---

- Analyse if the files are still there.

As you can see the files are not here, since we have not defined a specific volume.
Because of this, the data gets saved in the container.
Above we removed our container = data is lost.

![[Pasted image 20240919141129.png]]

![[Pasted image 20240919141410.png]]

---

- Stop the container (this should also remove the container since you used the `--rm` flag)

Exactly, our container was deleted automatically when it got stopped.

![[Pasted image 20240919141856.png]]

---

- Define a volume inside the container, adding this line to the Dockerfile: `VOLUME ["/app/persistent"]`

With the specified volume, files are not saved in the container-filesystem, they are saved in a volume ouside of the container.

new dockerfile:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base  
USER $APP_UID  
WORKDIR /app  
EXPOSE 8080  
EXPOSE 8081  
  
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build  
ARG BUILD_CONFIGURATION=Release  
WORKDIR /src  
COPY ["Docker_Ex5/Docker_Ex5.csproj", "Docker_Ex5/"]  
RUN dotnet restore "Docker_Ex5/Docker_Ex5.csproj"  
COPY . .  
WORKDIR "/src/Docker_Ex5"  
RUN dotnet build "Docker_Ex5.csproj" -c $BUILD_CONFIGURATION -o /app/build  
  
FROM build AS publish  
ARG BUILD_CONFIGURATION=Release  
RUN dotnet publish "Docker_Ex5.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false  
  
FROM base AS final  
WORKDIR /app  
COPY --from=publish /app/publish .  

# added here
VOLUME ["/app/persistent"]  
  
ENTRYPOINT ["dotnet", "Docker_Ex5.dll"]
```

---

- Rebuild the image

![[Pasted image 20240919142647.png]]

---

- Inspect the image and check if the volume is defined

```shell
docker inspect volumes-exercise
```

Here we can see, that there is a volume '/app/persistent'

![[Pasted image 20240919143038.png]]

---

- Start a new container from the image using `docker run -d -p 8080:8080 --rm --name volumescontainer volumes-exercise`
    
    - Note: In recent Images provided by Microsoft, the App runs under the user **app**, yet if you create a volume as described above, the created folder will be owned by the root user. Therefore, the application won’t have write access to this folder. To change the ownership of that folder add these lines to the Dockerfile:
        
        ```fallback
          VOLUME ["/app/persistent"]  
          RUN mkdir /app/persistent  
          RUN chown app /app/persistent
        ```

new Dockerfile:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base  
USER $APP_UID  
WORKDIR /app  
EXPOSE 8080  
EXPOSE 8081  
  
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build  
ARG BUILD_CONFIGURATION=Release  
WORKDIR /src  
COPY ["Docker_Ex5/Docker_Ex5.csproj", "Docker_Ex5/"]  
RUN dotnet restore "Docker_Ex5/Docker_Ex5.csproj"  
COPY . .  
WORKDIR "/src/Docker_Ex5"  
RUN dotnet build "Docker_Ex5.csproj" -c $BUILD_CONFIGURATION -o /app/build  
  
FROM build AS publish  
ARG BUILD_CONFIGURATION=Release  
RUN dotnet publish "Docker_Ex5.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false  
  
FROM base AS final  
WORKDIR /app  
COPY --from=publish /app/publish .  

# added here  
VOLUME ["/app/persistent"]  
RUN mkdir /app/persistent    
RUN chown app /app/persistent  
  
ENTRYPOINT ["dotnet", "Docker_Ex5.dll"]
```

Also make sure to rebuild the image, since the dockerfile has been changed.

```shell
docker build -t volumes-exercise -f Docker_Ex5/Dockerfile .
```

![[Pasted image 20240919144006.png]]

---

- Again use the API and check if the files are created accordingly.

Post:

![[Pasted image 20240919144118.png]]

Get:

![[Pasted image 20240919144256.png]]

/temp & /persistent are available.



![[Pasted image 20240919144448.png]]

---

- Use Docker desktop to analyse if a volume has been created.
    
- Stop the container (thus removing it).
    
- Check if the volume is still there
    
- Start a new container and analyse if the files are still there.
    
- Stop the container (thus removing it).
    
- Start a new container from the image using `docker run -d -p 8081:8080 -v myvolume:/app/persistent --rm --name volumescontainer volumes-exercise`
    
- Generate data
    
- Stop and remove the container
    
- Check if the volume is still there
    
- Start a new container from the image using `docker run -d -p 8081:8080 -v myvolume:/app/persistent --rm --name volumescontainer volumes-exercise`
    
- Check if the files are still there.