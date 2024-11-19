#Software-Engineering 

---
## # Python File Operation

A file is a container in computer storage devices used for storing data.

When we want to read from or write to a file, we need to open it first. When we are done, it needs to be closed so that the resources that are tied with the file are freed.

Hence, in Python, a file operation takes place in the following order:

1. Open a file
2. Read or write (perform operation)
3. Close the file
---
#### # Opening Files in Python

In Python, we use the `open()` method to open files.

To demonstrate how we open files in Python, let's suppose we have a file named `test.txt` with the following content.

```test.txt
This is test file.
Hello from the test file.
```

#### # Code for opening data ("r")

This creates a file object "file1". This object can be used to work with files/directories

```python
# open file in current dir
file1 = open("test.txt")
```

By default, the files are open in **read mode** (cannot be modified). The code above is equivalent to:

```python
file1 = open("test.txt", "r")
```

Here, we have explicitly specified the mode by passing the `"r"` argument which means file is opened for reading.

---
#### # Different modes to open files 

|Mode|Description|
|---|---|
|`r`|Open a file for reading. (default)|
|`w`|Open a file for writing. Creates a new file if it does not exist or truncates the file if it exists.|
|`x`|Open a file for exclusive creation. If the file already exists, the operation fails.|
|`a`|Open a file for appending at the end of the file without truncating it. Creates a new file if it does not exist.|
|`t`|Open in text mode. (default)|
|`b`|Open in binary mode.|
|`+`|Open a file for updating (reading and writing)|

Examples

```python
file1 = open("test.txt")      # equivalent to 'r' or 'rt'
file1 = open("test.txt",'w')  # write in text mode
file1 = open("img.bmp",'r+b') # read and write in binary mode
```

---
### # Reading Files

After we open a file, we use the `read()` method to read its contents.
Also make sure to `close()` the file properly to free up resources. For example, 

```python
# open a file
file1 = open("test.txt", "r")

# read the file
read_content = file1.read()
print(read_content)

# close the file properly
file1.close()
```

**Output**

```test.txt
This is a test file.
Hello from the test file.
```

---
#### # Use of with...open Syntax

In Python, we can use the `with...open` syntax to automatically close the file. For example,

```python
with open("test.txt", "r") as file1:
    read_content = file1.read()
    print(read_content)
```

**Note**: Since we don't have to worry about closing the file, make a habit of using the `with...open` syntax.

---
### # Writing Files

There are two things we need to remember while writing to a file.

- If we try to open a file that doesn't exist, a new file is created.
- If a file already exists, its content is erased, and new content is added to the file.

In order to write into a file in Python, we need to open it in write mode by passing `"w"` inside `open()` as a second argument.

Suppose, we don't have a file named **test2.txt**. Let's see what happens if we write contents to the `test2.txt` file.

```python
with open('test2.txt', 'w') as file2:

    # write contents to the test2.txt file
    file2.write('Programming is Fun.')
    fil2.write('Hello World! C# > Python')
```

Here, a new `test2.txt` file is created and this file will have contents specified inside the `write()` method.

**Output**

```test2.txt
Programming is Fun.
Hello World! C# > Python.
```
---
#### # Different file methods

There are various methods available with the file object. Some of them have been used in the above examples.

Here is the complete list of methods in text mode with a brief description:

|Method|Description|
|---|---|
|close()|Closes an opened file. It has no effect if the file is already closed.|
|detach()|Separates the underlying binary buffer from the `TextIOBase` and returns it.|
|fileno()|Returns an integer number (file descriptor) of the file.|
|flush()|Flushes the write buffer of the file stream.|
|isatty()|Returns `True` if the file stream is interactive.|
|read(n)|Reads at most n characters from the file. Reads till end of file if it is negative or `None`.|
|readable()|Returns `True` if the file stream can be read from.|
|readline(n=-1)|Reads and returns one line from the file. Reads in at most n bytes if specified.|
|readlines(n=-1)|Reads and returns a list of lines from the file. Reads in at most n bytes/characters if specified.|
|seek(offset,from=`SEEK_SET`)|Changes the file position to offset bytes, in reference to from (start, current, end).|
|seekable()|Returns `True` if the file stream supports random access.|
|tell()|Returns an integer that represents the current position of the file's object.|
|truncate(size=`None`)|Resizes the file stream to size bytes. If size is not specified, resizes to current location.|
|writable()|Returns `True` if the file stream can be written to.|
|write(s)|Writes the string s to the file and returns the number of characters written.|
|writelines(lines)|Writes a list of lines to the file.|

---
 
#### Working with paths

```python
import os

current_file = os.path.realpath('file_io.ipynb')  
print('current file: {}'.format(current_file))
```

---
#### Note: in .py files you can get the path of current file by __file__

```python
current_dir = os.path.dirname(current_file)  
print('current directory: {}'.format(current_dir))
```

----
#### Note: in .py files you can get the dir of current file by os.path.dirname(__file__)

```python
data_dir = os.path.join(os.path.dirname(current_dir), 'jupyterNotebooks')
print('data directory: {}'.format(data_dir))
```

---
#### Checking if path exists
```python
print('exists: {}'.format(os.path.exists(data_dir)))
print('is file: {}'.format(os.path.isfile(data_dir)))
print('is directory: {}'.format(os.path.isdir(data_dir)))
```

---
## Reading files

```python
file_path = os.path.join(data_dir, 'simple_file.txt')

with open(file_path, 'r', encoding="UTF-8") as simple_file:
    for line in simple_file:
        print(line.strip())
```

The [`with`](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement) statement is for obtaining a [context manager](https://docs.python.org/3/reference/datamodel.html#with-statement-context-managers) that will be used as an execution context for the commands inside the `with`. Context managers guarantee that certain operations are done when exiting the context. 

In this case, the context manager guarantees that `simple_file.close()` is implicitly called when exiting the context. This is a way to make developers life easier: you don't have to remember to explicitly close the file you openened nor be worried about an exception occuring while the file is open. Unclosed file maybe a source of a resource leak. Thus, prefer using `with open()` structure always with file I/O.

To have an example, the same as above without the `with`.
```python
file_path = os.path.join(data_dir, 'simple_file.txt')
```


### THIS IS NOT THE PREFERRED WAY

```python
simple_file = open(file_path, 'r')
for line in simple_file:
    print(line.strip())
simple_file.close()  # This has to be called explicitly 
```

## Writing files

```python
new_file_path = os.path.join(data_dir, 'new_file.txt')

with open(new_file_path, 'w') as my_file:
    my_file.write('This is my first file that I wrote with Python.')
# Now go and check that there is a new_file.txt in the data directory. After that you can delete the file by:
if os.path.exists(new_file_path):  # make sure it's there
    os.remove(new_file_path)
```
