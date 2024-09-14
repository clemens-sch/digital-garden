#Technologies 

---

In this exercise you will write your own Dockerfiles to dockerize 2 applications.
## # Preparation

Copy the following files into a local directory on your system. Use the paths specified above the files.

python-app/bmi.py

```python
print('(1) Metric (m, kg) or (2) Non-Metric (ft, pounds)?')

chosen_system = input('Please choose: ')

if (chosen_system != '1' and chosen_system != '2'):
  print('You have to choose either metric or non-metric. Shutting down...')
  exit()

height_unit = 'meters'
weight_unit = 'kilograms'

if (chosen_system == '2'):
  height_unit = 'feet'
  weight_unit = 'pounds'

print('Please enter your height in ' + height_unit)
user_height = float(input('Your height: '))

print('Please enter your weight in ' + weight_unit)
user_weight = float(input('Your weight: '))

adj_height = user_height
adj_weight = user_weight

if (chosen_system == '2'):
  adj_height = user_height / 3.28084
  adj_weight = user_weight / 2.20462

bmi = adj_weight / (adj_height * adj_height)

print('Your body-mass-index: ' + str(bmi))
```

node-app/server.js

```javascript
const express = require('express');

const app = express();

app.get('/', (req, res) => {
  res.send(`
    <h1>Hello from inside the very basic Node app!</h1>
  `);
})

app.listen(3000);
```

node-app/package.json

```json
{
  "name": "app",
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

## # Instructions

Dockerize BOTH apps - the Python and the Node app.

1. Create appropriate images for both apps (two separate images!). HINT: Have a brief look at the app code to configure your images correctly!
    
2. Launch a container for each created image, making sure, that the app inside the container works correctly and is usable.
    
3. Re-create both containers and assign names to both containers. Use these names to stop and restart both containers.
    
4. Clean up (remove) all stopped (and running) containers, clean up all created images.
    
5. Re-build the images - this time with names and tags assigned to them.
    
6. Run new containers based on the re-built images, ensuring that the containers are removed automatically when stopped.
