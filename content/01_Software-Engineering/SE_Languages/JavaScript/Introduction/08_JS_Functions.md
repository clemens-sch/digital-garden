#Software-Engineering 

---
## # What is a function in JS?

it is a reusable block of code/programming statement to perform a certain task.
a function can return a value or not.

Declaration for a function:
- function-key-word + function-name + "(_parameter_)"

---
### # Why using a Function?

functions are:
1. clean & easy to read
2. reusable
3. easy to test

---
### # Some Ways of Creating a Function

- Declaration function
- Expression function
- Anonymous function
- Arrow function

---
### # Writing a Function

#### # Declaration Function

Note (for unlimited number of parameters):
- a function declaration provides a function scoped `arguments` array like object. Anything we pass as argument in the function can be accessed from the arguments object inside the function.

```js
// Example: without a parameter -- 4
function square() {
	let num = 2
	let sq = num * num
	console.log(sq)
}
square()

// Example: function returning a value --  Clemens
function printName(){
	let firstName = 'Clemens'
	let space = ' '
	let printNameNew = space + firstName
	return printNameNew
}
console.log(printName())

// This way you can also create function with parameters
// Example: take array is parameter -- 631
function sumArrayValues(arr) {
    let sum = 0;
    for (let index = 0; index < arr.length; index++) {
      sum += arr[index]
    }
    console.log(sum)
}
const numbers = [1, 3, 4, 8, 9, 111, 11, 40, 444]
sumArrayValues(numbers);

// Example: function with unlimited number of parameters -- 10
function sumAllNumbers() {
    let sum = 0;
    for (let index = 0; index < arguments.length; index++) {
        sum += arguments[index]        
    }
    return sum
}
console.log(sumAllNumbers(1, 2, 3, 4))
```
---
#### # Anonymous Function

Anonymous function/without name

```js
// value is stored in a variable
const anonymousFun = function() {
  console.log(
    'I am an anonymous function and my value is stored in anonymousFun'
  )
}
```
---
#### # Expression Function

these are anonymous functions.
after we create a function without a name and we assign it to a variable.
to return value from the function we should call the variable.

```js
const square = function(n) {
	return n * n
}

console.log(square(2)) // --> 4
```
---
#### # Self Invoking Function

these are anonymous functions.
they do not need to be called to return a value.

```js
;(function (n) {
    console.log(n*n)
})(2) // --> 4
```
---
#### # Arrow Function

is an alternative to write a function - but there are some minor differences.
- arrow functions use `=>` instead of keyword `function` to get declared.
- after the declaration there is the `(a, b, ...)` parameterlist.

```js
const square = (n) => {
    return n * n;
}
console.log(square(4)) // --> 16

// This could also be changed into a one-liner (because we only have one line of code in code-block)
const oneLineSquare = (n) => n * n 
console.log(oneLineSquare(4)) // --> 16

const changeToUpperCase = (arr) => {
    const newArr = [];
    newArr.push(arr.toUpperCase())
    return newArr;
}
console.log(changeToUpperCase("hello this is an array.")) // --> HELLO THIS IS AN ARRAY.

const printFullName = (fn, ln) => fn + " " + ln
console.log("🚀 ~ printFullName:", printFullName('Clemens', 'Schmid')) // --> Clemens Schmid
```
---
### # Function with default value

sometimes we pass default values to parameters.
- when the function gets invoked & we don't pass an argument, the default value will be used.

```js
function defaultValueFunction(age = 18, birthYear = 2005) {
    return age + " " + birthYear
}
console.log(defaultValueFunction()) // --> 18 2005
console.log(defaultValueFunction(34, 1990)) // --> 34 1990

const weightOfObject = (mass, gravity = 9.81) => {
    const weight = mass * gravity + " N";
    return weight
}
console.log("🚀 ~ weightOfObject:", weightOfObject(5)) // --> 49.05000...
console.log("🚀 ~ weightOfObject:", weightOfObject(5, 1.62)) // --> 8.10000...
// Note: 1.62 is the gravity on the moon
```
---
---