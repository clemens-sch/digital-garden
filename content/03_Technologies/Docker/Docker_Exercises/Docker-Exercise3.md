#Technologies 

---

Get to know the following commands. Play around with your Docker environment and try out all the commands.
## # Tag an image

Create an image and give a name to it. Name is a group of images and tag further specifies the application.

```bash
docker build -t name:tag .
```

For example:

```bash
docker build -t helloworld
docker build -t helloworld:latest
docker build -t helloworld:v3
```

The tag latest will be added as a default if you don’t specify a tag yourself.

## # Give a container a name

```bash
docker run -p 3000:80 -d --rm --name helloworldcontainer helloworld
```

-p … Port forwarding -d … detached –rm remove the container after it is stopped –name give the container a name

## # Managing images and containers

```bash
# list running containers
docker ps

# list running and stopped containers
docker ps -a

# stop a running container
docker stop id_or_name

# remove a stopped container
docker rm id_or_name

# remove an image
docker rmi id

# remove all unused images (images where no container is running)
docker image prune

# automatically remove a running container when it exits
docker run --rm some_image

# see information about image
docker image inspect id
```
