#Software-Engineering 

---
## # Why using Conditionals?

To make decisions based on different conditions. 
Executed: from top ⇾ bottom

There are two ways an execution can be altered:
 1. **conditional execution**: statements are executed _if_ a expression is true.
 2. **repetive execution**: statements are repetitively executed as long as expression is true. 

---
### # Implementation

```js
if
if else
if else if else
switch
ternary operator
```

---
### # Examples

#### # If Statement

```js
let num = 3
if (num > 0) {
  console.log(`${num} is a positive number`)
}
//  3 is a positive number
```
---
#### # If-else Statement

```js
let num = -3
if (num > 0) {
  console.log(`${num} is a positive number`)
} else {
  console.log(`${num} is a negative number`)
}
//  -3 is a negative number
```
---
#### # If else-if else Statement

```js
let a = 0
if (a > 0) {
  console.log(`${a} is a positive number`)
} else if (a < 0) {
  console.log(`${a} is a negative number`)
} else if (a == 0) {
  console.log(`${a} is zero`)
} else {
  console.log(`${a} is not a number`)
}
```
---
#### # Switch Statement

```js
let weather = 'cloudy'
switch (weather) {
  case 'rainy':
    console.log('You need a rain coat.')
    break
  case 'cloudy':
    console.log('It might be cold, you need a jacket.')
    break
  case 'sunny':
    console.log('Go out freely.')
    break
  default:
    console.log(' No need for rain coat.')
}
```
---
#### # Ternary operator

Here we replace
if(true) { ... }
else ...

```js
let isRaining = true
isRaining
  ? console.log('You need a rain coat.')
  : console.log('No need for a rain coat.')
```

Here the variable name is "Pedro", when the age is greater than 10

```js
let age = 10;
let name = age > 10 && "Pedro"
// This would be the opposite
let name = age > 10 || "Pedro"
```

```js
let age = 16;
let name = age > 10 ? "Pedro" : "Jack";
```

---