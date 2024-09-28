#Software-Engineering 

---
## # .map()

Creates a new array by applying a given function to every element in an existing array.
It does not modify the original array but instead returns a new array with transformed values.

```js
let numbers = [1, 2, 3, 4, 5];
let doubled = numbers.map(num => num * 2);
```

```js
let users = [
	{ name: "John", age: 56},
	{ name: "Roman", age: 50}
];
let username = users.map(user =>
	user.name);
```

---
## # .filter()

Creates a new array containing elements, that pass a specific condition. 
When the condition returns true, the element is included in the new array.

```js
let numbers = [1, 2, 3, 4, 5];
let newArr = numbers.filter(num =>
	num > 3);
```

---
## # .reduce()

Used to "reduce" an array to a single value by executing a reducer function (that we provide). 
This results in a final result.

```js
let numbers = [1, 2, 3, 4, 5];
// note: it starts with 0, since accumulator (acc) = 0
// Then it takes: 0 + 1 + 2 + ...
let sum = numbers.reduce((acc, curr) => acc + curr, 0);
```

