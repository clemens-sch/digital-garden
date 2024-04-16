#Software-Engineering 

---
## What are Datatypes?

We use datatypes to store a specific value in a so called `variable` or whatever...

Data can be divided into two:
- primitive datatypes
- non-primitive datatypes

### # Primitive
can be compared - TRUE/FALSE

| primitive type | datatype                                                |
| ------------------ | --------------------------------------------------- |
| Numbers            | Integers, Floats                                    |
| Strings            | any data under ' ' or " " or ` `` `                 |
| Booleans           | true or false value                                 |
| Null               | no value or empty value                             |
| Undefined          | declared variable without value                     |
| Symbol             | unique value - can be created by Symbol constructor |

---
### # Non-Primitive
can't be compared - FALSE

| non-primitive type |
| ------------------ |
| Objects            |
| Arrays             |
| Functions          | 

```JavaScript
let nums = [1, 2, 3]
nums[0] = 10

console.log(nums)  // [10, 2, 3]
```

---
#### # Numbers

represents integers & decimal values 

##### # Math Object
provides lots of methods to work with numbers

```JavaScript
const PI = Math.PI; // variable for PI(3.1415...)

console.log(Math.round(PI)); // 3 - rounds down
console.log(Math.round(9.81)) // 10 - rounds up

console.log(Math.floor(PI)) // 3 - rounds down
console.log(Math.ceil(PI)) // 4 - rounds up

console.log(Math.min(-5, 3, 20, 4, 5, 10)) // -5, returns the minimum value
console.log(Math.max(-5, 3, 20, 4, 5, 10)) // 20, returns the maximum value

const randNum = Math.random() * 11 // creates random number between 0 to 10

//Absolute value
console.log(Math.abs(-10))      // 10

//Square root
console.log(Math.sqrt(100))     // 10

// Power
console.log(Math.pow(3, 2))     // 9

console.log(Math.E)             // 2.718

// Logarithm
// Returns the natural logarithm with base E of x, Math.log(x)
console.log(Math.log(2))        // 0.6931471805599453
console.log(Math.log(10))       // 2.302585092994046

// Returns the natural logarithm of 2 and 10 respectively
console.log(Math.LN2)           // 0.6931471805599453
console.log(Math.LN10)          // 2.302585092994046

// Trigonometry
Math.sin(60)
Math.cos(60)
```

#### # Strings

represent text 
- for declaring we need ` " ", ' ', `` `

```JavaScript
// 2 back-ticks for template strings
console.log(`hello ${randomNum}`)
// else - interpreted as basic string
console.log("hello ${randomNum}")
```

##### # String Methods
Each character in a string can be accessed by using its index. 
Every Index starts with 0 - ends with length of string -1

```JavaScript
let string2 = 'I love JavaScript. If you do not love JavaScript what else can you love.'
let string1 = 'JavaScript'
let string = '  30 Days Of JavaScript  '

console.log(string1.length) // index of string - 9

console.log(string1.toUpperCase())     // JAVASCRIPT
console.log(string1.toLowerCase())     // javascript

console.log(string1.substr(4,6))    // Script
console.log(string1.substring(3))      // aScript


console.log(string.split())     // Changes to an array -> ["30 Days Of JavaScript"]
console.log(string.split(' '))  // Split to an array at space -> ["30", "Days", "Of", "JavaScript"]

console.log(string.trim(' ')) // "30 Days Of JavaScript"

console.log(string.includes('Days'))     // true
console.log(string.includes('days'))     // false - it is case sensitive!

console.log(string.replace('JavaScript', 'C#')) // 30 Days Of C#

console.log(string.charAt(0))        // 3
console.log(string.charCodeAt(3))        // D ASCII number is 68
console.log(string.indexOf('D'))          // 3
console.log(string2.lastIndexOf('JavaScript')) // 38

let string3 = '30'
console.log(string3.concat("Days", "Of", "JavaScript")) // 30DaysOfJavaScript

console.log(string2.startsWith('I'))   // true
console.log(string2.endsWith('love.'))         // true

console.log(string2.search('love'))          // 2

let string4 = 'love'
console.log(string4.repeat(10)) // lovelovelovelovelovelovelovelovelovelove
```

---
#### # Checking data types & Casting

##### # Check data types
To check data types, we use `typeof` method.

```JavaScript
let test = 'sample'             // string
let age = 250                   // number
let job                         // undefined

console.log(typeof(test)) // string
console.log(typeof(age))  // number
console.log(typeof(job))  // undefined
console.log(typeof null)  // object
```

##### # Casting
Convert one data type to another
We use `parseInt(), parseFloat(), Number()`

```JavaScript
// String --> Int
let num = '10'

let numInt = parseInt(num)
console.log(numInt) // 10

let numInt1 = Number(num)
console.log(numInt1) // 10

let numInt2 = +num
console.log(numInt2) // 10


// String --> Float
let num1 = '9.81'
let numFloat = parseFloat(num1)

... Number(...)

... = + ...

// Float --> Int
let num2 = 9.81
let numInt = parseInt(num2)
console.log(numInt) // 9
```

---