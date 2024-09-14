#Software-Engineering 

[Main-Source](https://numpy.org/doc/stable/user/quickstart.html)

---
## # Introduction

This is a short overview of arrays in NumPy. It demonstrates how n-dimensional (n >= 2) arrays are represented & can be manipulated.

**Import - NumPy**

```python
import numpy as np
```

---
### # Create Array

**basic method**

```python
# 1D
arr = np.array([2.1, 3, 4, 16.4])

# multi-dimension
arr = np.arry([1, 2, 3], [4, 5, 6], ...)
```

**advanced method**

```python
arr = np.zeros((3, 4))
# Output:
array([[0., 0., 0., 0.],
       [0., 0., 0., 0.],
       [0., 0., 0., 0.]])

arr = np.ones((2, 3, 4), dtype=np.int16)
# Output:
array([[[1, 1, 1, 1],
        [1, 1, 1, 1],
        [1, 1, 1, 1]],

       [[1, 1, 1, 1],
        [1, 1, 1, 1],
        [1, 1, 1, 1]]], dtype=int16)

arr = np.empty((2, 3))   # random numbers - normally: 'float64'
array([[3.73603959e-262, 6.02658058e-154, 6.55490914e-260],
       [5.30498948e-313, 3.14673309e-307, 1.00000000e+000]])
```

---
### # Important Basic Attributes

```python
# show contents of array
arr

# number of axes (dimensions) of the array
arr.ndim
# Output: 1

# returns count of rows & columns
arr.shape
# Output: (4,)

# returns array with modified shape
arr.reshape(2, 2)
# Output: 
array([[ 2.1,  3. ],
       [ 4. , 16.4]])
   # arr.reshape again for converting back to 1D dimension

# total number of elements in array
arr.size
# Output: 4

# type of elements in array
arr.dtype
# Output: ('float64')

# elements: type 'float64' = 64/8 itemsize; 'complex32' = 32/8 itemsize
arr.itemsize
# Output: 8

# memory address: where element is saved in array (normally: use indexing)
arr.data
# Output: <memory at 0x7fab1405c100>
```
---
### # Basic Functions

```python
# create array with regularly spaced values within a range
# (START, STOP, STEPS)
np.arange(5, 50, 5)
# Output: array([ 5, 10, 15, 20, 25, 30, 35, 40, 45])
```
---
### # Print Array

```python
a = np.arange(6)   # 1D array
print(a)
# Output: [0 1 2 3 4 5]
```
---
### # Array Operations

```python
# +/-
a = np.array([10, 20, 30, 40])
b = np.arange(4)
c = a - b
c
# Output: array([10, 19, 28, 37])

# ^n
b**2
# Output: array([0, 1, 4, 9])

# Bool
a < 35
# Output: array([ True,  True,  True, False])

# .max()

# .min()

# .sum()
```

---
