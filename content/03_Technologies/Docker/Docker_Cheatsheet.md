#Technologies

---
## # Docker Shell-Commands

Here is a list of the most important docker commands.

### # Docker build Container

| flag | definition                          |
| ---- | ----------------------------------- |
| -t   | give image a name                   |
| -f   | finds dockerfile in specific folder |

```shell
# build image - Output: ... => => writing image sha256:307e51e493e71a
docker build .

docker build -t imagewithname .
docker build -t helloworld:v3 .

# if .dockerfile is in other folder
docker build -t mynewimage -f /Folder/Dockerfile .
```

### # Docker run Container

| flag   | definition                           |
| ------ | ------------------------------------ |
| -p     | port forwarding                      |
| -it    | enter internal terminal              |
| -d     | detached - allows writing in cmd     |
| -v     | defines a volume                     |
| --rm   | remove container, after it's stopped |
| --name | give container a name                |

```shell
docker run -p 1000:3000 307e51e

docker run -it node

docker run -d -p 3000:3000 --rm --name mynewcontainer mynewimage

# anonymous volume
docker run -v /app/data ...
# named volume
docker run -v data:/app/data ...
# bind mount (windows - "/path/to/code:/app/code")
docker run -v /path/to/code:/app/code
```

### # Docker basic Commands

```shell
# list running containers
docker ps
# list running and stopped containers
docker ps -a

# stop container
docker stop id_or_name

# connect with container bash in internal terminal
docker exec -it name bash

# remove a stopped container
docker rm id_or_name
# remove an image
docker rmi id
# remove all unused images (images where no container is running)
docker image prune

# see information about image
docker image inspect id
```

---
## # Dockerfile

Example of a basic `dockerfile` (node)

```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "node", "app.mjs" ]
```

Example from the .NET-world

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
  
VOLUME ["/app/persistent"]  
RUN mkdir /app/persistent    
RUN chown app /app/persistent  
  
ENTRYPOINT ["dotnet", "Docker_Ex5.dll"]
```
