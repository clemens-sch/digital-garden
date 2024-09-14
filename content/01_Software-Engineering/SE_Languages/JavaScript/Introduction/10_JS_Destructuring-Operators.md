#Software-Engineering 

---
## # What means Destructuring ?

Unpack arrays, objects & assign them to a distinct variable.
It allows us to write clean & readable code.

In JS, we can destructure
- Arrays
- Objects

---
## # Destructuring arrays

Arrays - list of different data types, which are ordered by their index.
During destructuring each variable should match with index of desired item in array. 

Example for accessing _small_ arrays: 

```js
const numbers = [1, 2, 3]

// access array - iterating via index over array
for (const number in numbers) {
	console.log(number)
}

// accessing array item manually
let num2 = numbers[1]
console.log(num2)
```

**Destructuring**

```js
const numbers = [1, 2, "hello world!"]
const [num1, num2, num3] = numbers
// { } - would also be valid
const {num1, num2, num3} = numbers
console.log(num1, num2, num3)
// O: 1 2 hello world!

// This will throw an undifined error - countries does not exist
const [fin, swe, nor, den] = countries
console.log(den) // undefined
```

**Destructuring & skipping an item: ,**

If we are not interested in destructuring an item we can leave it empty with a single ` ,` at that specific index.

```js
const numbers = [1, 2, "hello world!"]
const [num1, , num3] = numbers
console.log(num1, num2, num3)
// O: 1 hello world!
```

**Destructuring the rest: ...

To get the rest of an array, we use the spread operator `...` to get the rest of the array during destructuring.

```js
const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
const [num1, num2, num3, ...rest] = nums
console.log(num1, num2, num3, rest) //1, 2, 3, [4, 5, 6, 7, 8, 9, 10]
```

**Destructuring Nested Array** 

```js
const fullstack = [
	["HTML", "CSS", "React"],
	["Node", "Express", "MongoDB"]
]
const [frontEnd, backEnd] = fullstack
console.log(frontEnd, backEnd)
// O: ["HTML", "CSS", "React"] , ["Node", "Express", "MongoDB"]
```

---
### # Use cases: Arrays

When working with arrays we can use array destructuring - when looping though arrays

```js
const countries = [
  ['Finland', 'Helsinki'],
  ['Sweden', 'Stockholm'],
  ['Norway', 'Oslo'],
]

for (const [country, city] of countries) {
  console.log(country, city)
}
/* O:
Finland, Helsinki
Sweden, Stockholm
Norway, Oslo
*/
```

**Destructuring during loop through array**

```js
const languages = [
  { lang: "English", count: 91 },
  { lang: "French", count: 45 },
  { lang: "Arabic", count: 25 },
];

for (const { lang, count } of languages) {
  console.log(`${lang} is spoken in ${count} countries.`);
}
```

---
## # Destructuring Objects

An object literal consists of key & value pairs.
We already know on how to access objects & their values.

Here is a simple example:

```js
// this is an object
const rectangle = {
	width: 20,
	height: 10,
}
```

Access the objects values

```js
let width = rectangle.width
// or
let height = rectangle[height]
```

**Access value using destructuring**

When we use destructuring, the name of the variable should be exactly the same as the key/property of the object.

```js
let { width, height } = rectangle
console.log(width, height, perimeter) // 20, 10, undefined

// we could also use a default value
let { width, height, perimeter = 200 } = rectangle
console.log(width, height, perimeter) // 20, 10, 200
```

**Destructuring Nested Objects**

Here we destructure step by step

```js
const props = {
  user: {
    firstName: "Asabeneh",
    lastName: "Yetayeh",
    age: 250,
  },
  post: {
    title: "Destructuring and Spread",
    subtitle: "ES6",
    year: 2020,
  },
  skills: ["JS", "React", "Redux", "Node", "Python"],
};
const {
  user: { firstName, lastName, age },
  post: { title, subtitle, year },
  skills: [skillOne, skillTwo, skillThree, skillFour, skillFive],
} = props;
```

But we also can destructure it in one step

```js
const {
  user: { firstName, lastName, age },
  post: { title, subtitle, year },
  skills: [skillOne, skillTwo, skillThree, skillFour, skillFive],
} = props;
```

---
### # Spread/Rest Operator

For example, if we have an existing object, and we do just want to change one property in this object, we can change this one property as follows:

```js
const person = {
	name: "Pedro",
	age: 20,
	isMarried: false
};

const person2 = {...person, name: "Jack"};
```

The same is there for arrays.
In this example, let's say we have an array filled up with some names. We want to add another name in another array.

```js
const names = ["Predo", "Jack", "Franz"];
const names2 = [...names, "Herbert"];
```

---
