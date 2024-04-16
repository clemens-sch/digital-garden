#Software-Engineering 

---
## # What is React ?

React is a JavaScript library. NOT A FRAMEWORK.
For building web-apps using components.

---
## # Basic Understanding

### # jsx 

React uses a syntax extension `.jsx` (JavaScript XML) of JavaScript.
We use this extension when mixing HTML & JS code.

### # DOM - Document Object Model

Can be though that every HTML-Element is a element of the DOM.

When working with React, it creates a virtual DOM.
In React, the DOM is updated automatically without a need for a refresh.
As we can see in the "Real DOM":

![[Pasted image 20240327155659.png]]

---
## # File- & Folder-Structure 

- node_modules: external packages, libraries the project depends on
- public: contains any public assets (fonts, images, videos, ...)
- src>assets - files are bundled and automatically refreshed in the app
	- src: main.jsx (main .js file), App.jsx (root-component)
	- assets: files, videos, ...
- index.html - main entrypoint of application (script: main.jsx)
- package.json - contains metadata about project

---
## # Components

Here we gonna create a first component.
This component will be exportet in our root-component.

```js
function Header(){
    return(
        <header>
            <h1>My website</h1>
        </header>
    );
}

export default Header
```

```js
import Header from "./Header";

function App() {
  return <Header />;
}

export default App;
```

---
## # JavaScript & HTML ?

To write you JS-code mixed up with HTML you have to use { _your js-code_}.

```js
const Footer = () => {
  return (
    <footer>
        <p>&copy; {new Date().getFullYear()} Your website name</p>
    </footer>
  )
}

export default Footer

// Output: © 2024 Your website name
```

---
## # jsx fragement

As you know you can only return one elemente in a .jsx file.
Therefore, we need a workaround.

We gonna use jsx fragements.

```js
import Header from "./Header";
import Footer from "./Footer";

function App() {
  return (
    <>
      <Header />
      <Footer />
    </>
  );
}

export default App;
```

With these changes, we are able to add multiple components into one component.

---
## # props

These are read-only properties, that are shared between components.
A parent component can share data to a child component. 

<Component key=value>This is a sample component.</Component>

```js
// Student component
function Student(props) {
    return (
        <div>
            <p>Name: {props.name}</p>
            <p>Age: {props.age}</p>
            <p>Student: {props.isStudent ? "Yes" : "No"}</p>
        </div>
    );
}

export default Student;

// App component
import Card from "./Card";
import Student from "./Student";

function App() {
  return (
    <>
      <Student name="Clemens" age={18} isStudent={true}/>
      <Card />
      <Card />
    </>
  );
}

export default App;
```

### # proptypes

This is a mechanism, that ensures that the passed value is of the correct datatype.
age: PropTypes.number

```js
// in our Student component
import PropTypes from 'prop-types';

Student.propTypes = {
    name: PropTypes.string,
    age: PropTypes.number,
    isStudent: PropTypes.bool
};
```

### # defaultProps

Specify a default value for props in case they are not passed from the parent component.
name: "Guest"

```js
Student.defaultProps = {
    name: "Guest",
    age: 0,
    isStudent: false
};
```

---
## # Conditional Rendering

This allows you to control what gets rendered in your application based on certain conditions (show, hide, change components).

We can resolve this using booleans & then displaying components.

---
## # Click-Event

An interaction when a user clicks on a specific element.
We can respond to clicks by passing a callback to the onClick event handler.

```js
const Button = () => {
  const handleClick = () => {
    console.log("OUCHHHH!");
  };
  return <button onClick={handleClick}>Click me</button>;
};

export default Button;
```

---
## # React hook

Special function, that allows functional components to use React features without writing class components.
- useState, useEffect, useContext, useReducer, useCallback, ....

If a function begins with an "use" it probably is a React hook.
### # useState()

This is a React hook, that allows the creation of a stateful variable AND a setter function to update its value in the virtual DOM.

"name, setName"

In this example, we have an array with 2 variables.
The name with the current state "name" and a "setName" to change the state/value, the "setName" will automatically transfer the text to the "name" variable.

```js
/* eslint-disable no-unused-vars */
import React, { useState } from "react";

function MyComponent() {
  const [name, setName] = useState("Guest");
  
  const updateName = () => {
    setName("Clemo");
  };

  return (
    <div>
      <p>Name: {name}</p>
      <button onClick={updateName}>Set Name</button>
    </div>
  );
}

export default MyComponent;
```

---
## # onChange 

Represents an event-handler primarly used with form elements.
- input, textarea, select, radio, ...

Triggers a function every time the value of the input changes.

```js
import { useState } from "react";

const MyComponent2 = () => {
  const [quantity, setQuantity] = useState(10);  

  function handleQuantityChange(event) {
    setQuantity(event.target.value);
  }

  return (
    <div>
      <input type="number" value={quantity} onChange={handleQuantityChange}/>
      <p>Quantity: {quantity}</p>
    </div>
  );
}; 

export default MyComponent2;
```

---
## # Update Function

A function passed as an argument to setState() usually.

```js
// example: if I want to setYear (year + 1) - instead write:
setYear(updater function)
```

! This allows for safe updates based on the previous state !

Used with multiple state updates & asynchronous functions.
Good practice to use updater functions.

When using the updater-function, it takes the PENDING state to calculate the NEXT state.
React puts your updater-function in a queue.
During the next render, it will call them in the same order.

```js
const Counter = () => {
  const [value, setValue] = useState(0);

  const handleDecrease = () => {
    setValue(v => v - 1);
    setValue(v => v - 1);
    setValue(v => v - 1);
  }
```

---
## # update Objects in State

```js
import React, { useState } from 'react'

const AnotherComponent = (): React.JSX.Element => {
    const [car, setCar] = useState({ year: 2024, make: "Ford", model: "Mustang" });

    function handleYearChange(event): void {
        // setCar({ ...car, year: event.target.value });
        setCar(c => ({ ...c, year: event.target.value }));
    }

    return (
        <div>
            <p>Your favorite car is: {car.year}, {car.make}, {car.model}</p>
            <input type="number" value={car.year} onChange={handleYearChange} /> <br />
            <input type="text" value={car.make} /> <br />
            <input type="text" value={car.model} /> <br />
        </div>
    )
}

export default AnotherComponent
```

---
## # update Arrays in State

```js

```

---