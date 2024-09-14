#Software-Engineering 

---
To handle data in an asynchronous way, we use async, await in combination with the Fetch-API.

To read more about async/await, read: [[SE_TPL - Async Await]]

---
## # Promises ?

This is an option for waiting for anything to be done.
Class when dealing with things, that need a short amount of time.
_BUT: We won't use it like this!_

```js
// resolve - success; reject - fail
const event = new Promise((resolve, reject) => {
	var name = "Pedro";
	name == "Pedro" ? resolve(name) : reject("Name != Pedro, it was " + name);
});

// .then() - called when .resolve() - if the data is received THEN
// .catch() - called when .reject()
// .finally() - called everytime - no matter if success/failure
// like a TRY-CATCH
event
	.then((name) => {
		console.log(name);
	})
	.catch((err) => {
		console.log(err);
	})
	.finally(() => {
		console.log("PROMISE FINISHED!")
	})
```

---
## # Real-World Example

For this GET-request, we use the fetch API.

```js
fetch('https://catfact.ninja/facts')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        // Displaying some content from the response
        const facts = data.data.map(fact => fact.fact);
        console.log(facts.slice(0, 5)); // Displaying the first 5 facts
    })
    .catch(error => {
        console.error('There was a problem with the fetch operation:', error);
    })
    .finally(() => {
      console.log("WE FINISHED")
    });
```

---
## # async/await ?

This is the replacement for promises.
When using this method, we no longer need the .then(), .catch(), .finally().

When we make the GET-request we wait until this request is finished. When we have to data we write it into the console.

```js
const axios = require("axios");

const fetchData = async() => {
	const data = await axios.get("https://catfact.ninja/facts");
	console.log(data);
}
```

### # Error-Handling

For the async/await we need a simple try-catch.

```js
const axios = require("axios");

const fetchData = async() => {
	try {
		const data = await axios.get("https://catfact.ninja/facts");
		console.log(data);
	}
	catch (err) {
		console.log(err)
	}
	finally {
		console.log("HELLO ANOTHER WORLD")
	}
};
```

---
## # Fetch-API

This is a builtin-browser function.
This method is promise-based. Meaning, that we are able to use async/await.

```js
fetch ("https://reqres.in/api/users")
	.then(res => res.json())
	.then(data => console.log(data))
```

---