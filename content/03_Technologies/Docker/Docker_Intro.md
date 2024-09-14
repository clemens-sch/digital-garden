#Definitions 

---
# # Docker

## # What is Docker?

Docker is a container technology: A tool for creating and managing containers.
With Docker always imagine using Linux.

---
## # Docker Engine

Is the technology behind docker. Is a software, which makes it possible to use kernel-operations for creating containers.
It also allows, that every container is seperated from others.

---
## # Container

A container is a standardized unit of software.

A container encapsulates code alongside with its dependencies that are needed to run the code.

For example a C# Application and the .NET Runtime.

---
## # Why?

Containers solve the problem of “it runs on my machine”. This happens when you develop an application, test it on your local environment (your development machine), deploy it to production and then something breaks, because some part of the environment is different than on your local machine.

**We want to build and test in exactly (!) the same environment as we later run our app in.**

---
## # Containers

- The same container always yields the **exact same application and execution behavior**! No matter where or by whom it might be executed.
- Support for Containers is **built into modern operating systems**.
- **Docker** simplifies the creation and management of containers.

---
## # Reliability and Reproducible Environments

- We want to have the **exact same environment for development and production** →\rightarrow→ This ensures that it works exactly as tested.
- It should be easy to **share a common development environment** with (new) colleagues.

---
## # Virtual Machines

![VM.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/VM.excalidraw.svg)

---
## # Virtual Machines (2)

| Pro                                                              | Con                                                                        |
| ---------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Separated environments                                           | Redundant duplication, waste of space                                      |
| Environment-specific configurations are possible                 | Performance can be slow, boot times can be long                            |
| Environment configurations can be shared and reproduced reliably | Reproducing on another computer/server is possible but may still be tricky |

---
## # Containers

![Containers.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/Containers.excalidraw.svg)

---
## # Containers (2)

| Docker Containers                                         | Virtual Machines                                               |
| --------------------------------------------------------- | -------------------------------------------------------------- |
| Low impact on OS, very fast, minimal disk space usage     | Bigger impact on OS, slower, higher disk space usage           |
| Sharing, re-building and distribution is easy             | Sharing, re-building and distribution can be challenging       |
| Encapsulate apps/environments instead of “whole machines” | Encapsulate “whole machines” instead of just apps/environments |

---
## # Install Docker

Check official sources for latest information and requirements: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

![DockerInstallation.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/DockerInstallation.excalidraw.svg)

---
## # Using Docker

There are several ways to interact with docker. Most of the features nowadays can conveniently be accessed through Docker Desktop.

However, we will focus on using the command-line interface (CLI) to better see and understand what is going on.

When running docker commands in the CLI always make sure that Docker Desktop is running.

Commands: [https://docs.docker.com/get-started/docker_cheatsheet.pdf](https://docs.docker.com/get-started/docker_cheatsheet.pdf)

---
## # Images vs Containers

| Images                                  | Containers                                            |
| --------------------------------------- | ----------------------------------------------------- |
| Templates/Blueprints for containers     | The running “unit of software”                        |
| Contains code + required tools/runtimes | Multiple containers can be created based on one image |

---
## # One Image Multiple Containers

![ImagesContainers.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/ImagesContainers.excalidraw.svg)

---
## # Build an image using a Dockerfile

The file itself is only called `dockerfile`.

Note: Every command in a dockerfile creates a new layer. Docker saves these layers in the cache. When a layer does not change docker reuses this layer from the cache.

```dockerfile
# Specify a base image (node - runtime for JavaScript) 
# (...:14 - image-version "tag")
FROM node:14

# Set the working directory inside the image
# Folder is only available in container
WORKDIR /app

# Copy the local file package.json 
# to the current (working) directory (which is /app) inside the image
COPY package.json .

# Run a command
# in .NET - NuGet: dotnet restore ...
RUN npm install

# Copy everything from the current directory of the host
# to the current (working) directory inside the image
COPY . .

# Document which port will be accessible (optional)
EXPOSE 3000

# Set the entrypoint of the container
# "command", "argument"
CMD [ "node", "app.mjs" ]
```

---
## # Build an image

Run this command inside the folder that contains your Dockerfile:

```bash
docker build .
```

enter a name for image (we can also enter a tag/"latest")

```shell
docker build -t myNodeApp:14 .
```

---
## # Run a container based on the image

```bash
docker run <image_name>
```

or

```bash
# -p port - for webapplication & accessing via browser
# -d for beeing able to write in cmd
docker run -p <host_port>:<container_port> -d <image_name>
```

---
## # Images are read-only

After building an image, changes to the source code files won’t be reflected inside the image. You have to build the image again in order to make changes to the image.

---
## # Multi stage Dockerfiles

### # .NET build & run

**SDK** - Tools for creating software application.
- "Software Development Kit"
- Creates .dll, .exe

**Runtime** - takes output (.ddl, .exe) & runs them.

With this method we don't need a SDK:

```dockerfile
FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
# /app - basic docker folder
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
# .csproj - contains connections (like package.json)
COPY ["HelloWorld/HelloWorld.csproj", "HelloWorld/"]
RUN dotnet restore "HelloWorld/HelloWorld.csproj"
# copy from host --> image
COPY . .
WORKDIR "/src/HelloWorld"
RUN dotnet build "HelloWorld.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "HelloWorld.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HelloWorld.dll"]
```

Are most commonly used to differ between build an run environments. `build` contains the SDK, `final` only contains the runtime.

---
## # Inspect Image

```shell
docker image inspect dockerex1
```

Container-Name basically has a Hash-value. Hash from filesystem.
- Hash does some mathematical operations
- Hash is one-line - can't be reconverted

---
## # .Dockerignore

For ignoring things in a dockerfile.

---
## # Interpreter vs Compiler

Interpreter: JavaScript
- has node:14, ...
Compiler: C#
- compiles files - generates .exe, .dll