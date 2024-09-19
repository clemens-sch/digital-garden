#Software-Engineering 

---
## # What are Scopes in JS?

When we declare variables in JS, we declare a variable at a different scope. 
Variables can be declared
- globally
- locally
- window scope

---
### # Types of Scopes

**global scope**

can be accessed everywhere in same file. 

```js
let a = 'JavaScript' // global scope
let b = 10 // global scope
function letsLearnScope() {
  console.log(a, b) // JavaScript 10, accessible
  if (true) {
    let a = 'Python'
    let b = 100
    console.log(a, b) // Python 100
  }
  console.log(a, b)
}
letsLearnScope()
console.log(a, b) // JavaScript 10, accessible
```
---

**local scope**

can only be accessed in certain block code.

```js
let a = 'JavaScript' // global scope
let b = 10 // global scope
function letsLearnScope() {
  console.log(a, b) // JavaScript 10, accessible
  let c = 30
  if (true) {
    // variables declared inside the if will not be accessed outside the if block
    let a = 'Python'
    let b = 20
    let d = 40
    console.log(a, b, c) // Python 20 30
  }
  // we can not access c because c's scope is only the if block
  console.log(a, b, c) // JavaScript 10 30
}
letsLearnScope()
console.log(a, b) // JavaScript 10, accessible
```
---

**window scope**

no explicit variable-type. accessible everywhere in the code

```js
a = 'JavaScript' // is a window scope this found anywhere
b = 10 // this is a window scope variable
function letsLearnScope() {
  console.log(a, b)
  if (true) {
    console.log(a, b)
  }
}
console.log(a, b) // accessible
```
---
### # Block Scoped

use `let`, `const`

```js
function letsLearnScope() {
  // you can use let or const, but gravity is constant I prefer to use const
  const gravity = 9.81
  console.log(gravity)
}
// console.log(gravity), Uncaught ReferenceError: gravity is not defined

if (true) {
  const gravity = 9.81
  console.log(gravity) // 9.81
}
// console.log(gravity), Uncaught ReferenceError: gravity is not defined

for (let i = 0; i < 3; i++) {
  console.log(i) // 1, 2, 3
}
// console.log(i), Uncaught ReferenceError: gravity is not defined
```

---
### # Scoped to function

use `var`
BUT: TRY TO AVOID `var`

```js
function letsLearnScope() {
  var gravity = 9.81
  console.log(gravity)
}
// console.log(gravity), Uncaught ReferenceError: gravity is not defined

if (true) {
  var gravity = 9.81
  console.log(gravity) // 9.81
}
console.log(gravity) // 9.81

for (var i = 0; i < 3; i++) {
  console.log(i) // 1, 2, 3
}
console.log(i)
```
