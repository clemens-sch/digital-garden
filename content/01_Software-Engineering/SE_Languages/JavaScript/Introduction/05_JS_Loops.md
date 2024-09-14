#Software-Engineering 

---
## # Why do we use loops?

To do repetitive tasks multiple times (automatically).
Loop goes until condition gets false.
- _break_ - interrupt loop.
- _continue_ - skip item during iteration.

---
### # Loops

```js
for
while
do while
for of
forEach
for in
```

---
### # Interrupting & skipping Loops

_break_ - interrupts a loop
_continue_ - skip certain iteration

```js
// code stops, if 3 found in iteration
for (let i = 0; i <= 5; i++) {
  if (i == 3) {
    break
  }
  console.log(i)
}
// 0 1 2
```

```js
// 3 is skipped
for (let i = 0; i <= 5; i++) {
  if (i == 3) {
    continue
  }
  console.log(i)
}
// 0 1 2 4 5
```

---
### # Examples

### # For Loop

when we know how many iterations we go.

```js
let sum = 0
for (let i = 0; i < 101; i++) {
  sum += i
}
```
---
#### # While Loop

when we don't specifically know how many iterations we go.

```js
let count = prompt('Enter a positive number: ')
while (count > 0) {
  console.log(count)
  count--
}
```
---
#### # Do-While Loop

runs at least once if condition is true/false.

```js
let count = 11
do {
  console.log(count)
  count++
} while (count < 11)
```
---
#### # For-of Loop

to iterate over an array/list ... for every item.

```js
const numbers = [1, 2, 3, 4, 5]
for (const number of numbers) {
  console.log(number)
}
```
---
#### # ForEach Loop

if we are interested in index of array. Can take 2 or 3 arguments: item, index, array.

```js
const numbers = [1, 2, 3, 4, 5]
numbers.forEach((number, i) => {
  console.log(number, i)
})

const countries = ['Finland', 'Sweden', 'Norway', 'Denmark', 'Iceland']
countries.forEach((country, i, arr) => {
  console.log(i, country.toUpperCase())
})
```
---
#### # For-in Loop

if we want to get object with specific key.

```js
const user = {
  firstName: 'Clemens',
  lastName: 'Schmid',
  age: 18,
  country: 'Austria',
  skills: ['HTML', 'CSS', 'JS', 'React'],
}

for (const key in user) {
  console.log(key, user[key])
}
```

---
