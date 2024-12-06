#Technologies 

---
## # Preparation

Copy the following files into a local directory on your system.

app.mjs

```javascript
import express from 'express';

import connectToDatabase from './helpers.mjs'

const app = express();

app.get('/', (req, res) => {
  res.send('<h2>Hi there!</h2>');
});

await connectToDatabase();

app.listen(3000);
```

helpers.mjs

```javascript
const connectToDatabase = () => {
  const dummyPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve();
    }, 1000);
  });

  return dummyPromise;
};

export default connectToDatabase;
```

package.json

```json
{
  "name": "docker-start",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

Dockerfile

```dockerfile
FROM node:14

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD [ "node", "app.mjs" ]
```

---
## # Build the image

Open a CLI and navigate into the folder you copied the files to.

Run:

```bash
docker build .
```

The output will give you a container Id. Something like

```bash
=> => writing image sha256:307e51e493e71a
```

---
## # Run the container based on the image

Use the first few characters of the ID (copy enough so that it is unique) to run a container based on that image.

```bash
docker run -p 3000:3000 307e51e
```

You can then open the app on port 3000 of your local machine.

---
## # Try using different ports

Specify different ports for both the exposed container port aswell as the host port.

```shell
# also edit dockerfile & app.mjs
# make sure host runs over port 3010
docker run -p 4010:3010 38c86108
```

---
## # Make a change

Change the HTML output from the web server to something else. Run the container again and make sure you see the changes.
- Here you also have to rebuild the container

---
## # Optimize the Dockerfile

Make sure that `npm install` only executes when the package.json file changes.

```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3010

CMD [ "node", "app.mjs" ]
```
