#Software-Engineering 

---
## # What are Objects in JS?

Everything can be an object. An object has properties & properties have values.
Object = key-value pair.
An object is a mutable data structure = content can be modified after it gets created.

### # Creating Objects

```js
const person = {
    lastName : "Schmid",
    isMarried : false,
    age : 18,
    skills : [
        "C#",
        "JavaScript",
        "RESTful API"
    ],
    getFullDetails : function() {
        return `${this.lastName} ${this.age} ${this.isMarried}`
    }      
}
```

---
### # Get Values from Object

```js
console.log(person.age)
console.log(person.isMarried)
console.log(person.getFullDetails())
/* Output
18
false
Schmid 18 false
*/
```

---
### # Setting new Keys in an Object

In this example we have to look at the person object, we created above

```js
person.age = 69
person.isMarried = true
person.skills.push("Python")
console.log(person)
/* Output
{lastName: 'Schmid', isMarried: true, age: 69, skills: Array(4), getFullDetails: f}
*/
```

---
### # Object Manipulation

#### # object.assign()

to copy an object without modifying the original object

```js
const copyPerson = Object.assign({}, person)
// Output
{
    "lastName": "Schmid",
    "isMarried": false,
    "age": 18,
    "skills": [
        "C#",
        "JavaScript",
        "RESTful API"
    ]
}
```

#### # object.keys()

to get the keys/properties of an object as array

```js
const keys = Object.keys(copyPerson)
// Output
[
    "lastName",
    "isMarried",
    "age",
    "skills",
    "getFullDetails"
]
```

#### # object.values()

to get object values

```js
const values = Object.values(copyPerson)
// Output
[
    "Schmid",
    false,
    18,
    [
        "C#",
        "JavaScript",
        "RESTful API"
    ],
    null
]
```

#### # object.entries

to get object keys & values

```js
const entries = Object.entries(copyPerson)
// Output
// (5) [Array(2), Array(2), Array(2), Array(2), Array(2)]
[
    [
        "lastName",
        "Schmid"
    ],
    [
        "isMarried",
        false
    ],
    [
        "age",
        18
    ],
    [
        "skills",
        [
            "C#",
            "JavaScript",
            "RESTful API"
        ]
    ],
    [
        "getFullDetails",
        null
    ]
]
```

---
#### # Check properties using hasOwnProperty()

to check if a specific key/property exists in an object.
- bool

```js
console.log(copyPerson.hasOwnProperty("skills"))
// Output
true
```

---
