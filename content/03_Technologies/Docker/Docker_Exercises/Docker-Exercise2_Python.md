#Technologies 

---
## # Connect to an internal shell

```bash
docker run -it node
```

---
## # Preparation

Copy the following files into a local directory on your system.

rng.py

```python
from random import randint

min_number = int(input('Please enter the min number: '))
max_number = int(input('Please enter the max number: '))

if (max_number < min_number): 
  print('Invalid input - shutting down...')
else:
  rnd_number = randint(min_number, max_number)
  print(rnd_number)
```

Dockerfile

```dockerfile
FROM python

WORKDIR /app

COPY . /app

CMD ["python", "rng.py"]
```

## # Build the image

```bash
docker build -t python_random .
```

## # Run a container

```bash
docker run -it python_random
```
