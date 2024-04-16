#Software-Engineering 

---
## # Built-in Data Types

### # Number

Represents integer & floating-point numbers.

```ts
let age: number = 25;
let price: number = 29.99;
let anotherNumber: number = 123_456_789;
```

### # String

Represent textual data.

```ts
let name: string = "John";
```

### # Boolean

Represents true/false values.

```ts
let isStudent: boolean = true;
```

### # Undefined

For variables, that have been declared but not assigned a value.

```ts
let undefinedVar: undefined;
```

### # Null

For variables, that clearly represents an absence of any object value.

```ts
let nullVar : null = null;
```

### # Any

Avoid using this type. With this type, you lose the static type feature.

```ts
let level;
level = 1;
// would be possible, the variable is considered as "any" type
level = "start";
```

### # Void

Return type of functions - that do not return a value.

```ts
function logMessage(): void {
	console.log("Hello, Typescript!");
}
```

---
## # User-Defined Data Types

### # Arrays

Allow you to store multiple values of the same type.

```ts
let numbers: number[] = [1, 2, 3, 4, 5, 6];
```

### # Enums

To organize & represent a set of named values.


```ts
// by default - .ts-compiler: Id of Red: 0, Green: 1, Blue: 2
// using a `const enum Color` - compiler generates more optimized code
enum Color {
	Red, 
	Green,
	Blue
}

let myColor: Color = Color.Green;
```

### # Classes

Provide a blueprint for creating objects with methods & properties.

```ts
class Person {
	constructor(public name: string) {}
}

let person = new Person("Alice");
```

### # Interfaces

Define the structure of objects & can be used to enforce a specific shape on objects.

```ts
interface Shape {
	area(): number;
}

class Circle implements Shape {
	constructor(private radius: number) {}
	area(): number {
		return Math.PI * this.radius ** 2;
	}
}
```

### # Tuple Data Type 

Allow you to express an array where the type of fixed number of elements is known.
Useful for key-value pairs.

```ts
let person: [string, number] = ["John", 30];
```

### # Object Data Type

Objects in TS can have a specific shape defined by interfaces or types.

```ts
interface Person {
	name: string;
	age: number;
}

let person: Person = { name: "John", age: 30 };
```

### # Custom Data Type

Cerating custom types using the `type` keyword.

```ts
type Point = { x: number; y: number };

let point: Point = { x: 10; y: 20 };
```

### # Class TypeScript

Classes are a fundamental part of oop in TypeScript. 

```ts
class Animal {
	constructor(public name: string) {}
	makeSound(): void {
		console.log("Some generic sound");
	}
}

let dog = new Animal("Bello");
dog.makeSound(); // Output: Some generic sound
```

 