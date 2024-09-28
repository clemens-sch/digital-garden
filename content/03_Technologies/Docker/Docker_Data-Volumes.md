#Technologies 

---
## # Types of data

![TypesOfData.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/TypesOfData.excalidraw.svg)

---
## # Container and Image Data

Note for layers: when the state of an Image in a .dockerfile changes - a new layer is created.

![ContainerImageData.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/ContainerImageData.excalidraw.svg)

---
## # Container Data

Data can be stored inside containers as files, just as you would normally do on your local development machine.

However, once a container is removed, the data will be lost.

> Stopping and restarting a container will not delete your data within that container.

---
## # Volumes

- Volumes are **folders on your host machine** hard drive which are **mounted** (“made available”, mapped) **into containers**.

- Volumes **persist if a container shuts down**. If a container (re-)starts and mounts a volume, any data inside of that volume is **available in the container**.
  Not only for restarting - when removing a container the data persists.
- A container **can write** data into a volume **and read** data from it.

---
## # Defining Volumes

```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# does nothing on technical layer (only for documentation purposes) 
EXPOSE 80

# docker sees this folder as volume
# if we don't write a docker run -v ... = automatically is anonymous volume
VOLUME [ "/app/feedback" ]

CMD ["node", "server.js"]
```

The **VOLUME** command specifies a path inside the container that will be treated as a volume. The mapping to the host’s file system is defined by the **run** command when starting a container.

---
## # Volumes & Bind Mounts

- **Anonymous Volume** - we don't know where the volume in the container is
	- <mark style="background: #D2B3FFA6;"> hardly used</mark>
- **Named Volume** - docker automatically allocates the data at a specific destination in the container 
	- <mark style="background: #D2B3FFA6;">mostly used</mark>
- **Bind Mount** - specifies the destination on your host-machine, which will also be the volume for the container
	- <mark style="background: #D2B3FFA6;">often used</mark>

```bash
# Anonymous Volume
docker run -v /app/data ...

# Named Volume
docker run -v data:/app/data ...

# Bind Mount
docker run -v /path/to/code:/app/code
```

---
## # Volumes vs. Bind Mounts

![VolumesBindmounts.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/VolumesBindmounts.excalidraw.svg)

---
## # Container / Volume Interaction

![ContainerVolumeInteraction.excalidraw.svg](https://deep-thought.norwin.at//tech-kb/containers/assets/ContainerVolumeInteraction.excalidraw.svg)

---
## # Summary

- Containers can read + write data. Volumes can help with data storage, Bind Mounts can help with direct container interaction.
- Containers can read + write data, but written data is lost if the container is removed.
- Volumes are folders on the host machine, managed by Docker, which are mounted into the container.
- Named Volumes survive container removal and can therefore be used to store persistent data.
- Anonymous Volumes are attached to a container - they can be used to save (temporary) data inside the container.
- Bind Mounts are folders on the host machine which are specified by the user and mounted into containers - like Named Volumes.
