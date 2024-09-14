#Software-Engineering 

---
## # What is an Error?

There are 2 distinguishable kinds of errors:

1. syntax errors
errors detected by compiler

```python
while True print('Hello world')
  File "<stdin>", line 1
    while True print('Hello world')
                   ^
SyntaxError: invalid syntax
# This error is caused: line 1: ":" is missing
```

arrow - points at earliest point, where error was detected


2. exceptions
errors detected during execution

```python
10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
4 + spam*3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'spam' is not defined
'2' + 2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

last line indicated what happened
exceptions come in different tyes
types in this example: `ZeroDivisionError`, `NameError`, `TypeError`

---
## # Handling Exceptions

it's possible to write programms that handle selected exceptions

### # try except 

`try`
tries something - if it does not work properly:
`except` clause with specific type is executed

```python
... except (RuntimeError, TypeError, NameError):
...     pass
```

Example - wants user to input a valid integer (user can interrupt program - ctrl-c)

```python
while True:
...     try:
...         x = int(input("Please enter a number: "))
...         break
...     except ValueError:
...         print("Oops!  That was no valid number.  Try again...")
```

----
### # raise

allows the programmer to force a specified exception to occur

Example - raise a custom type exception 

```python
try:
...     raise NameError('HiThere')
... except NameError:
...     print('An exception flew by!')
...     raise
...
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: HiThere
```

---
### # User-defined Exceptions

Exceptions should be derived from `Exception` class, either directly/indirectly
An Exception should end with the word "Error"

---