#Technologies

---
## # Docker Shell-Commands

Here is a list of the most important docker commands.

### # Docker build Container

| flag | definition        |
| ---- | ----------------- |
| -t   | give image a name |

```shell
# build image - Output: ... => => writing image sha256:307e51e493e71a
docker build .

docker build -t imagewithname .
docker build -t helloworld:v3 .
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

# named volume
docker run -v data:/app/data ...
# bind mount
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

Example of a basic `dockerfile`

```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "node", "app.mjs" ]
```

