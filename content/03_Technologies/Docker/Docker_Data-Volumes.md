#Technologies 

[6-min summary](https://www.youtube.com/watch?v=p2PH_YPCsis&t=21s)

---
## Types of data

![TypesOfData.excalidraw.svg](https://deep-thought.norwin.at/_astro/typesofdataexcalidraw.BdbiialS_1utMgb.svg)

---

## Container and Image Data

![ContainerImageData.excalidraw.svg](https://deep-thought.norwin.at/_astro/containerimagedataexcalidraw.3Kv1vvEt_Z1P0W5t.svg)

---

## Container Data

Data can be stored inside containers as files, just as you would normally do on your local development machine.

However, once a container is removed, the data will be lost.

Note

Stopping and restarting a container will not delete your data within that container.

---

## Volumes

- Volumes are **folders on your host machine** hard drive which are **mounted** (“made available”, mapped) **into containers**.

![[Pasted image 20241110195125.png]]

- Volumes **persist if a container shuts down**. If a container (re-)starts and mounts a volume, any data inside of that volume is **available in the container**.
- A container **can write** data into a volume **and read** data from it.

---

## Defining Volumes

```
FROM node:14
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 80
VOLUME [ "/app/feedback" ]
CMD ["node", "server.js"]
```

The **VOLUME** command specifies a path inside the container that will be treated as a volume. The mapping to the host’s file system is defined by the **run** command when starting a container.

---

## Volumes & Bind Mounts

Terminal window

```
# Anonymous Volumedocker run -v /app/data ...
# Named Volumedocker run -v data:/app/data ...
# Bind Mountdocker run -v /path/to/code:/app/code
```

---

## Volumes vs. Bind Mounts

![VolumesBindmounts.excalidraw.svg](https://deep-thought.norwin.at/_astro/volumesbindmountsexcalidraw.B7sF5eqs_sPWso.svg)

---

## Container / Volume Interaction

![ContainerVolumeInteraction.excalidraw.svg](https://deep-thought.norwin.at/_astro/containervolumeinteractionexcalidraw.BitDGX8T_1nY975.svg)

---

## Summary

- Containers can read + write data. Volumes can help with data storage, Bind Mounts can help with direct container interaction.
- Containers can read + write data, but written data is lost if the container is removed.
- Volumes are folders on the host machine, managed by Docker, which are mounted into the container.
- Named Volumes survive container removal and can therefore be used to store persistent data.
- Anonymous Volumes are attached to a container - they can be used to save (temporary) data inside the container.
- Bind Mounts are folders on the host machine which are specified by the user and mounted into containers - like Named Volumes.