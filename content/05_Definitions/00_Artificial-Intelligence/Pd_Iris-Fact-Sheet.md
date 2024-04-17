#Definitions 

---
## # What is the Iris-DataSet ?

The Iris dataset includes 150 flowers, categorizing them
- based on four measurements: sepal length, sepal width, petal length, and petal width.

---
### # Task-Preparation

```python
import pandas as pd
import sklearn as skl
from sklearn import datasets
# Loading Iris DataSet
Iris_data = datasets.load_iris()
```

---
### # Tasks

#### # Commands

```python
# gets names of attributes in DataSet ("data", "target", "features", ...)
Iris_data.keys() 
# gets target names from Iris DataSet (species of flowers)
Iris_data['target_names']
```

---

`unique` - displays unique values in a column
- in our example: displays only the species: 0, 1, 2