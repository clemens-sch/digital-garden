#Software-Engineering 

---
## # What are Higher Order Functions ? 

take other function as a parameter or return a function as value.
the function passed as parameter is called **callback**.

---
### # Callback

A callback can be passed as parameter to other functions

```js
// a callback function, the function could be any name
const callback = (n) => {
  return n ** 2;
};

// function takes other function as a callback
function cube(callback, n) {
  return callback(n) * n;
}
console.log(cube(callback, 3));
```

Output: 27

---
### # Return Function

Higher order functions return a function as a value

```js
// Higher order function returning an other function
const higherOrder = (n) => {
  const doSomething = (m) => {
    const doWhatEver = (t) => {
      return 2 * n + 3 * m + t;
    }
    return doWhatEver
  }
  return doSomething
}
console.log(higherOrder(1)(2)(3))
```

Output: 11

For example the forEach method uses callback:

```js
const numbers = [1, 2, 3, 4, 5, 6];
const sumArray = (arr) => {
  let sum = 0;
  const callback = function (element) {
    sum += element;
  };
  arr.forEach(callback);
  return sum;
};
console.log(sumArray(numbers));
```

Output: 21

**A simpler option for the example above would be**

```js
const numbers = [1, 2, 3, 4]
​
const sumArray = arr => {
  let sum = 0
  arr.forEach(function(element) {
    sum += element
  })
  return sum
}
console.log(sumArray(numbers))
```

Output: 21

---
### # Setting Time

In JS we can execute some activity for a specific time. 
We can wait for something to execute.

#### # setIntervall

we use this higherOrderFunction to repeatedly execute a specified function at fixed time intervals.

```js
function sayHello() {
  console.log('Hello')
}
// This print "Hello" every 2 seconds
setInterval(sayHello, 2000)
```

#### # setTimeout

we use this higherOrderFunction to execute a specified function after a fixed time **only once**.

```js
function sayHello() {
  console.log('Hello')
}
// This prints "Hello" only once after 2 seconds
setTimeout(sayHello, 2000) 
```

---
---