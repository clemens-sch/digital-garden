#Software-Engineering 

---
## # What are Arrays?

To compare them with variables: `array` can store multiple values.
Each value has own index.
Each index has a reference in a memory address.
Allows storing duplicate elements & different data types.
Is modifiable.

### # Sample arr examples

array with one data type
```js
const numbers = [0, 3.14, 9.81, 37, 98.6, 100] // array of numbers

// print array & .length
console.log('Numbers:', numbers)
console.log('Number of numbers:', numbers.length) // 5
```

```output
Numbers: [0, 3.14, 9.81, 37, 98.6, 100]
Number of numbers: 6
```

array with different data types
```js
const arr = [
  'Clemens',
  1000,
  true,
  { country: 'Austria', city: 'Vienna' },
  { skills: ['HTML', 'CSS', 'JS', 'React'] },
] // arr containing different data types
console.log(arr)
```

array within an array
```js
const frontEnd = ['HTML', 'CSS', 'JS', 'React', 'Redux']
const backEnd = ['Node', 'Express', 'MongoDB']
const fullStack = [frontEnd, backEnd]
console.log(fullStack) // [["HTML", "CSS", "JS", "React", "Redux"], ["Node", "Express", "MongoDB"]]
```

### # Access array items
You can access an item in an array with index
- starting from 0 - .length -1

![[Pasted image 20231210163004.png]]

```js
const fruits = ['banana', 'orange', 'mango', 'lemon']
let firstFruit = fruits[0] // access first index

console.log(firstFruit) // banana
```

### # Modifying array items
Once an array is created, we can modify the contents of array elements.

```js
const numbers = [1, 2, 3, 4, 5]
numbers[0] = 10 // changing 1 at index 0 to 10
numbers[1] = 20 // changing  2 at index 1 to 20

console.log(numbers) // [10, 20, 3, 4, 5]
```

### # array manipulating methods
Here are some available methods

```js
// ARRAY()
const arr = Array() // creates empty array
console.log(arr)
const eightEmptyValues = Array(8) // creates eight empty values
console.log(eightEmptyValues) // [empty x 8]

// .FILL()
const four4values = Array(4).fill(4) // creates 4 element values filled with '4'
console.log(four4values) // [4, 4, 4, 4]

// .CONCAT()
const firstList = [1, 2, 3]
const secondList = [4, 5, 6]
const thirdList = firstList.concat(secondList)
console.log(thirdList) // [1, 2, 3, 4, 5, 6]

// .LENGTH
const numbers = [1, 2, 3, 4, 5]
console.log(numbers.length)

// .INDEXOF()
const numbers = [1, 2, 3, 4, 5]
console.log(numbers.indexOf(5)) // -> 4

// .LASTINDEXOF()
const numbers = [1, 2, 3, 4, 5, 3, 1, 2]
console.log(numbers.lastIndexOf(2)) // 7
console.log(numbers.lastIndexOf(0)) // -1

// .INCLUDES
const numbers = [1, 2, 3, 4, 5]
console.log(numbers.includes(5)) // true
console.log(numbers.includes(0)) // false

// ARRAY.ISARRAY()
const numbers = [1, 2, 3, 4, 5]
console.log(Array.isArray(numbers)) // true
const number = 100
console.log(Array.isArray(number)) // false

// .TOSTRING()
const numbers = [1, 2, 3, 4, 5]
console.log(numbers.toString()) // 1,2,3,4,5

// .JOIN()
const names = ['Asabeneh', 'Mathias', 'Elias', 'Brook']
console.log(names.join()) // Asabeneh,Mathias,Elias,Brook
console.log(names.join('')) //AsabenehMathiasEliasBrook
console.log(names.join(' ')) //Asabeneh Mathias Elias Brook

// .SLICE()
const numbers = [1, 2, 3, 4, 5]
console.log(numbers.slice()) // -> it copies all items
console.log(numbers.slice(1, 4)) // -> [2,3,4] // it doesn't include the ending position

// -SPLICE()
const numbers = [1, 2, 3, 4, 5]
console.log(numbers.splice(0, 1)) // remove the first item
const numbers = [1, 2, 3, 4, 5, 6]
console.log(numbers.splice(3, 3, 7, 8, 9)) // -> [1, 2, 3, 7, 8, 9] //it removes three item and replace three items

// .PUSH()
const numbers = [1, 2, 3, 4, 5]
numbers.push(6)
console.log(numbers) // -> [1,2,3,4,5,6]

// .POP()
const numbers = [1, 2, 3, 4, 5]
numbers.pop() // -> remove one item from the end
console.log(numbers) // -> [1,2,3,4]

// .SHIFT()
const numbers = [1, 2, 3, 4, 5]
numbers.shift() // -> remove one item from the beginning
console.log(numbers) // -> [2,3,4,5]

// .UNSHIFT()
const numbers = [1, 2, 3, 4, 5]
numbers.unshift(0) // -> add one item from the beginning
console.log(numbers) // -> [0,1,2,3,4,5]

// .REVERSE()
const numbers = [1, 2, 3, 4, 5]
numbers.reverse() // -> reverse array order
console.log(numbers) // [5, 4, 3, 2, 1]

// .SORT()
const webTechs = [
  'HTML',
  'CSS',
  'JavaScript',
  'React',
  'Redux',
  'Node',
  'MongoDB',
]
webTechs.sort()
console.log(webTechs) // ["CSS", "HTML", "JavaScript", "MongoDB", "Node", "React", "Redux"]
```

---
### # useful array methods

- .map()
- .filter()

Okay, so why do we need these 2 functions?
Image we have an array with string-names.

The .map() function will go through each element in an array.

```js
let names = ["Pedro", "Jack", "Jessica"];

names.map((name) => {
	// each name will be outputed
	console.log(name);
})
```

And we want to add a "1" at the end of each name. So we gonna use the .map() method to override each element.

```js
names.map((name) => {
	return name + "1";
})
```


Also, the .filter() function loops through every element in an array.

```js
let names = ["Jessica", "Carol", "Pedro"];

names.filter((name) => {
	return name !== "Pedro"
});
```

---