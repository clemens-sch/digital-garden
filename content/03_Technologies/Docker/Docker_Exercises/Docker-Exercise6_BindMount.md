#Technologies 

---

Use the following files to dockerize a node application:

server.mjs

```javascript
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("<h2>Hi there! You hey</h2>");
});

app.listen(process.env.PORT);
```

package.json

```json
{
  "name": "data-volume-example",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "license": "ISC",
  "scripts": {
    "start": "nodemon server.mjs"
  },
  "dependencies": {
    "body-parser": "^1.19.0",
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "2.0.4"
  }
}
```

Dockerfile

```Dockerfile
FROM node:14
WORKDIR /app
COPY package.json .

RUN npm install
COPY . .

ARG DEFAULT_PORT=80
ENV PORT $DEFAULT_PORT
EXPOSE $PORT
CMD [ "npm", "start" ]
```

.dockerignore

```fallback
node_modules
Dockerfile
.git
```

.env

```fallback
PORT=8000
```

---
## # Instructions

Your goal is to have a docker container running that uses your most recent files on your host system as the application code that will be executed when you visit the web page running in the container. This way you can have a well-defined execution environment (using docker for managing the dependencies an environment), but still be able to make rapid changes to your code and test it out while developing.

- Build an image using the Dockerfile. Give it a tag of `bindmountimg`

```shell
docker build -t bindmountimg .
```

![[Pasted image 20241004141539.png]]

---

- Run a container based on the image using `docker run -it -p 8082:80 -v <absolute path to your working directory on the host machine>:/app -v /app/node_modules --rm --name bindmountcontainer bindmountimg`

```shell
docker run -it -p 8082:80 -v "C:/Users/Clemens/OneDrive - HTL Krems/HTL/5CHIT/SYT/SYTD/Docker-Ex/Ex6":/app -v /app/node_modules --rm --name bindmountcontainer bindmountimg
```

![[Pasted image 20241004142104.png]]

---

- Access the site in your browser

Under the URL: http://localhost:8082

![[Pasted image 20241004142140.png]]

---

- Make changes to your code and change the HTML output of your site.

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.send(
        "<h2>Hi there! I'm using WhatsApp!</h2>" +
            "<h1>I have a bigger font!?</h1>"
    );
});

app.listen(process.env.PORT);
```

---

- Refresh the page in the browser

Note: In Windows also type `rs` in the console to refresh the container.

```shell
rs
```

![[Pasted image 20241004142717.png]]

---

- You should see your updated view.

![[Pasted image 20241004142734.png]]

---
### # Answering Questions

- How is the container port on which the application listens defined and managed?

The container listens on the port defined by the `process.env.PORT`:

```.env
PORT=8000
```

```dockerfile
FROM node:14
WORKDIR /app
COPY package.json .

RUN npm install
COPY . .

# define build-variable (only in docker-build process available)
ARG DEFAULT_PORT=80
# sets 80 as default port
ENV PORT $DEFAULT_PORT
# tells docker, which port is used in container
EXPOSE $PORT
CMD [ "npm", "start" ]
```

---

- What does the `ARG` command do in your Dockerfile, and how can you use that when building an image?

It defines a build-time-variable, that can only be used while the image-builds.
The value can be overwritten by the command `docker build --build-arg DEFAULT_PORT=3000 -t myapp .`

Example: We want to create a docker-image for an application. 
- In development, it runs on port 3000
- In production, it runs on port 80

```shell
# dev
docker build --build-arg PORT=3000 -t myapp-dev .

# prod
docker build --build-arg PORT=80 -t myapp-prod .
```

---

- Why does the application listen on the port defined in the environment variable?

The value `process.env.PORT` is used to configure the server on `app.listen(...)`
- If we want to change the port, we don't have to modify the code (only edit `.env` file)

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.send(
        "<h2>Hi there! I'm using WhatsApp!</h2>" +
            "<h1>I have a bigger font!?</h1>"
    );
});

app.listen(process.env.PORT);
```

---

- How can you set environment variables when running a container? (There are 2 ways. One might be connected to the .env file. But you should try it out to make sure.)

1. .env file
   
```.env
PORT = 8000
...
```

2. with docker run command

```shell
# -e variable=...
docker run -it -p 8083:80 -v "C:/Users/Clemens/OneDrive - HTL Krems/HTL/5CHIT/SYT/SYTD/Docker-Ex/Ex6":/app -v /app/node_modules -e PORT=9000 --rm --name bindmountcontainer bindmountimg
```

---

- What is a `.dockerignore` file?

Specifies files, that should be excluded from a docker image when it's built (can be compared with a `.gitignore`)
- Image sizes smaller
- Avoids copying unnecessary files

```dockerignore
node_modules
Dockerfile
.git
```

---

- What is `nodemon` and how did we use it in that example?

"Node" & "monitor"

nodemon is a utility, that automatically monitors changes in a Node.js app & automatically restarts the server when file-changes are detected.
- Note: On Windows you have to type `rs` to refresh the files
- eliminates the need for manual restarts

In our case, we used `nodemon` to run the `server.mjs` file through the start script in package.json, enabling automatic restarts whenever code is modified.

package.json:

```json
{
    "name": "data-volume-example",
    "version": "1.0.0",
    "description": "",
    "main": "server.js",
    "license": "ISC",
    "scripts": {
        // runs "nodemon server.mjs"
        "start": "nodemon server.mjs"
    },
    "dependencies": {
        "body-parser": "^1.19.0",
        "express": "^4.17.1"
    },
    "devDependencies": {
        "nodemon": "2.0.4"
    }
}
```

---

- Which part of the `docker run` command defines a bind mount?

```shell
docker run -it -p 8082:80 -v "C:/Users/Clemens/OneDrive - HTL Krems/HTL/5CHIT/SYT/SYTD/Docker-Ex/Ex6":/app -v /app/node_modules --rm --name bindmountcontainer bindmountimg
```

In this case, the `docker run ... -v ["host_path"]:[container_path] ...` defines the path on your host-computer, that you want to mount.
Then the directory on the host file system is shared with the container.

---

- Why did we have to specify an anonymous volume using `-v /app/node_modules`? What would happen if we left that out? (This last question is probably the most important question for you to understand how volumes and bind mounts interact with each other.)

Mounting the entire `/app` folder from the host could overwrite the `node_modules` in the container by the version of the host.
By using an anonymous volume for our `node_modules`, docker keeps the dependencies isolated from other updates.
- Allows updating the app code & makes sure, that the dependencies remain stable
