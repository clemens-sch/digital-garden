#Software-Engineering 

---
## # Which Variables exist in JS?

For variables, we use `let`, `var`, `const`

| type  | scope           | definition |
| ----- | --------------- |------------|
| let   | block scope     | can be reassigned| 
| const | block scope     | can't be reassigned|
| var   | function scope - old; avoid it | hoised to the top of their scope|

```js
let firstName = 'Asabeneh'
firstName = 'Eyob'

const PI = 3.14 // Not allowed to reassign PI to a new value
// PI = 3.
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
TRY TO AVOID `var`

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

---
