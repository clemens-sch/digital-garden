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



---

- Run a container based on the image using `docker run -it -p 8082:80 -v <absolute path to your working directory on the host machine>:/app -v /app/node_modules --rm --name bindmountcontainer bindmountimg`
- Access the site in your browser
- Make changes to your code and change the HTML output of your site.
- Refresh the page in the browser
- You should see your updated view.
- Try to answer the following questions:
    - How is the container port on which the application listens defined and managed?
    - What does the `ARG` command do in your Dockerfile and how can you use that when building an image?
    - Why does the application listen on the port defined in the environment variable?
    - How can you set environment variables when running a container? (There are 2 ways. One might be connected to the .env file. But you should try it out to make sure.)
    - What is a `.dockerignore` file?
    - What is `nodemon` and how did we use it in that example?
    - Which part of the `docker run` command defines a bind mount?
    - Why did we have to specify an anonymous volume using `-v /app/node_modules`? What would happen if we left that out? (This last question is probably the most important question for you to understand how volumes and bind mounts interact with each other.)