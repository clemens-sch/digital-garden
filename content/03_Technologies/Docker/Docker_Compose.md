#Technologies 

---
## # Docker Compose ?

Docker Compose is an **additional tool**, offered by the Docker ecosystem, which helps with **orchestration / management** of multiple Containers.

It can also be used for single Containers to simplify building and launching.

---
## # Why?

```bash
docker network create shop
docker build -t shop-api .
docker run -v logs:/app/logs --network shop --name shop-backend shop-api
docker build -t shop-database .
docker run -v data:/data/db --network shop --name shop-db shop-database
```

This is a very simple example - yet you got **quite a lot of commands** to execute and memorize to bring up all Containers required by this application.

And you have to run (most of) these commands whenever you change something in your code or you need to bring up your Containers again for some other reason.

With Docker Compose, this gets much easier.  
You can put your Container configuration into a **docker-compose.yaml** file and then use just one

command to bring up the entire environment: `docker-compose up`

---
## # Docker Compose Files

In docker compose files you can describe **services** (= your containers). For each service you can configure several options, such as:
- Published Ports
- Environmental Variables
- Volumes
- Networks

Basically you can configure anything that you can also specify using the `docker run` command, because docker-compose is meant as a substitution for that.

---
## # Docker Compose Files (2)

A `docker-compose.yaml` file looks like this:

```yaml
# only for documentation - version of the docker-compose file
version: "3.8" # version of the Docker Compose spec

# "Services" are in the end the Containers 
# that your app needs
# here are 2 services: "web", "db"
services: 
	web:
		# Define the path to your Dockerfile 
		# for the image of this container
		build: 
			context: .
			dockerfile: Dockerfile-web

		# Define any required volumes / bind mounts
		volumes: 
			- logs:/app/logs

	db:
		# image in local registry - if we don't have it, it pulls it
		image: 'mysql'
		volumes:
			- data:/data/db
```

---
## # Docker Compose Files (3)

You can conveniently edit docker-compose.yaml files at any time, and you just have a short, simple command which you can use to bring up your Containers:

```bash
docker-compose up
```

You can find the full list of configurations here: [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)
> When using Docker Compose, you automatically get a Network for all your Containers - so you don’t need to add your own Network unless you need multiple Networks!

---
## # Important commands

```bash
# Start all containers/services mentioned 
# in the Docker Compose file
docker-compose up
```

| flag    | definition                                                                                               |
| ------- | -------------------------------------------------------------------------------------------------------- |
| -d      | start in detached mode                                                                                   |
| --build | force docker compose to rebuild all images (otherwise, it only does that if an image is missing)         |
| -v      | remove all volumes used for containers (otherwise they stand around, even if the containers are removed) |

```bash
# Stop and remove all containers / services  
docker-compose down
```

Of course, there are more commands. You can have a look at the official command reference: [https://docs.docker.com/compose/reference/](https://docs.docker.com/compose/reference/)

---
## # What Docker Compose is NOT

Docker Compose: How docker should be running.
Dockerfile: Configure the docker container.

- Docker Compose does **NOT replace Dockerfiles** for custom images
- Docker Compose does **NOT replace Images or Containers**
- Docker Compose is **NOT suited** for managing multiple containers on **different hosts** (machines).

---
## # docker-compose.yaml - Version

```yaml
version: "3.8"
```

- This is the first (yet optional) element in the `docker-compose.yaml` file. It is for documentation purposes only. Docker Compose won’t interpret your file differently depending on the version. It will only issue warnings or errors when something can’t be understood.    
- You can use this information then to find out for which version this was written and what changed in the meantime to update your `docker-compose.yaml` file.

---
## # docker-compose.yaml - services

```yaml
version: "3.8"

services:
	app:
	db:
```

- Under `services:` you specify the containers that should run when starting your application. You can list one or more containers. Services is the only required element within the `docker-compose.yaml` file.
    
- The name (in this example `app` and `db`) can be chosen by you, the maintainer of the file. It loosely corresponds to the name for the container that will be started.
    
---
## # docker-compose.yaml -image

```yaml
services:
	db:
		image: "mysql:8.2.0"
```

- You can use the `image` element below a service to define which image should be use for the container.
    
- For this, the image has to be available in the (local or remote) registry.
    
---
## # docker-compose.yaml - build

```yaml
services:
	web:
		build:
			context: .
			dockerfile: ./WebApp/Dockerfile
```

- Use the `build` element below a service when you don’t have a pre-existing image, but will build it on demand.
    
- `context` is the folder from which you want to issue the build command. It should be the root directory of all files, that should potentially be copied into the image when building.
  Here the `docker build` would take place. 
    
- `dockerfile` is the location and name of the Dockerfile. A relative path is resolved from the build context.
    
---
## # docker-compose.yaml - ports

```yaml
services:
	web:
		image: "mywebapp"
		ports:  
			- "8080:8080"
			- "8081:8081"
```

.yaml-format: `-` specifies an array.

- Use the `ports:` element below a service to define which ports should be mapped to the host. For internal (container-to-container) communication you don’t have to define a port mapping.
- The syntax is basically the same as if you would issue a `docker run` command: `":"

---
## # docker-compose.yaml - volumes

```yaml
services:
	db:
		image: "mysql:8.2.0"
		volumes:  
			- dbdata:/var/lib/mysql

volumes:
	dbdata:
```

- Use the `volumes` element below a service to define volumes (named, anonymous or bind mounts).
- You can specify multiple volumes for each service
- The syntax for adding one volume element is the same as for `docker run` commands.
- For bind mounts you can and should use relative paths, though.
- If you define a named volume, you additionally have to add a `volumes` section (same level as services), where you add just the names of your your named volumes as sub elements.

---
## # docker-compose.yaml - environment

```yaml
services:
	db:
		image: "mysql:8.2.0"
		environment:  
			MYSQL_DATABASE: mydb  
			MYSQL_USER: sytd  
			MYSQL_PASSWORD: sytd  
			MYSQL_ROOT_PASSWORD: secret
```

- Use the `environment` element to define values for environment variables, the container supports.

```yaml
# Alternative syntax:
environment:
	- MYSQL_DATABASE=mydb
```

- Or you point to an external `.env` file holding the configuration values:

```yaml
env_file:
	- ./env/mysql.env
```

---
## # docker-compose.yaml - depends_on

```yaml
services:
	web:
		# ...
		depends_on:
			- db
	db:
		# ...
```

> Here at first, docker waits until the container "db" is instanciated.

- If you have dependencies between your containers (e.g. the web application needs a running db container before it can start), you can define them using the `depends_on:` element.

> `depends_on` only waits for the container to be running. That doesn’t necessarily mean, the service within the container is already responsive

**Fixtures**
- set a timeout (retry-logic)
- health-checks (command, if a container is "healthy")
	- in combination with `depends_on` - only starts, when container is "healthy"
