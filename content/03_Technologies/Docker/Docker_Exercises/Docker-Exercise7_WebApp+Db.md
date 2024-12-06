#Technologies 

---

Your goal in this exercise is to create a simple web application that stores its data within a database. Both the web application and the database should run in **2 separate** containers and communicate with each other.

![TodosApp.excalidraw.svg](https://deep-thought.norwin.at/_astro/todosappexcalidraw.BFM0qLPs_Z28pQw8.svg)

Tip

You will have to restart your containers several times during this exercise. To make sure, the image for your **TodosApp** will be rebuild every time, use the command `docker-compose up -d --build` (`-d` for detached and `--build` to force a rebuild of images when something changes).

---
## # Hints

You will probably stumble upon one problem if you try to containerize your app regarding database initialization. Up until now you probably followed the following steps for adding a database:

- Define your model
- Create a DbContext
- Add a migration
- Update the schema of your database
- Run the application

If you follow these steps, you will have to make sure, that running `dotnet ef database update` on your host system has access to your (already running) database container.

Another approach that you could look into is automatically running any pending database migrations from your **TodosApp** when the app starts. You can use the following code snippet in your `Program.cs` file to achieve this:

```javascript
var scope = app.Services.CreateScope();
using var dbContext = scope.ServiceProvider.GetRequiredService<TodoDbContext>();
var connected = false;
while (!connected)
{
  if (!dbContext.Database.CanConnect())
  {
    await Task.Delay(1_000);
    continue;
  }

  dbContext.Database.Migrate();
  connected = true;
}
```

---
## # Assignment

- Create your own Web Application using _AspNetCore_ (Blazor is recommended) and name it **TodosApp**.
- Create a `docker-compose.yaml` file and describe your **TodosApp**.

```yaml
version: "3.8"
services:
	web:
		build:
			context: ./ToDoApp/ 
			dockerfile: ./ToDoApp/Dockerfile 
		ports:
			- "6500:8080" 
			- "6501:8081"
```

- Use `docker-compose` to run your application in a container.

```bash
docker-compose up -d
```

- Check if the container is running (use the CLI **and** Docker Desktop).
    - Note how docker compose names your containers!

Naming:
1. Cluster: dockerexercise7
	1. Image: dockerexercise7-web
		1. container: web-1

![[Pasted image 20241205193348.png]]

- Check if your application is accessible through the specified port in your `docker-compose.yaml`

![[Pasted image 20241205200114.png]]

- Use `docker-compose` to stop and remove your running containers and check if it was successful.

```yaml
docker-compose down
```

- Your app should be capable of storing and retrieving Todos inside a database. The web view could look like this:

![[Pasted image 20241205200242.png]]

- You should be able to add single todo items by specifying a **Title**.
	- x
- You should be able to remove Todo Items (either single items, or all at once).
	- x
- You can choose different approaches for adding the database.
    - Implement your database connection completely on your host system. This requires an accessible database. (either on your host system, or in a docker container with an exposed port)
	    - x
    - Define 2 services in your `docker-compose.yaml` from the start and make sure the containers can communicate with each other

docker-compose.yaml:

```yaml
version: "3.8"
services:
  web:
    build:
      context: ./ToDoApp/
      dockerfile: ./ToDoApp/Dockerfile
    environment:
      -ConnectionStrings__DefaultConnection=Server=db;Database=todo;User=root;Password=password;
    ports:
      - "6500:8080"
      - "6501:8081"
    depends_on:
      - db
  db:
    image: mysql:8.0.21
    environment:
      MYSQL_DATABASE: todo
      MYSQL_USER: root
      MYSQL_PASSWORD: password
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - db-data:/var/lib/mysql
volumes:
  db-data:

```

dockerfile:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ToDoApp/ToDoApp.csproj", "ToDoApp/"]
RUN dotnet restore "ToDoApp/ToDoApp.csproj"

COPY . .
WORKDIR "/src/ToDoApp"
RUN dotnet build "ToDoApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ToDoApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ToDoApp.dll"]
```

- Use an existing image from docker hub for your database. E.g. `mysql:8.2.0`
	- x
- Make sure that your data is persisted through restarts of your application (using `docker-compose down` and `docker-compose up`).
	- x
- Set any necessary (or useful) environment variables for your database (check the documentation on docker hub).

.env:

```.env
MYSQL_DATABASE=todo
MYSQL_USER=root
MYSQL_PASSWORD=password
MYSQL_ROOT_PASSWORD=password
```

.docker-compose.yaml

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
  db:
    image: mysql:8.0.21
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    volumes:
      - db-data:/var/lib/mysql
volumes:
  db-data:

```

- Eventually make sure that both services (TodosApp and your database) are containerized and can be started through `docker-compose`
	- x
