#Software-Engineering 

---
## # What is TypeScript ?

TypeScript is a programming language created by microsoft. 
- built on top of JavaScipt
It addresses shortcomings of Vanilla JavaScipt. 

So, everything you can do in JS - you also can do in TS.

You can think of it like a bubble.

![[Pasted image 20240408165752.png]]

---
## # TypeScript Features

Some features TypeScript supports, that JS does not.
- Static typing (number, string, ...)
- TS compiler - at compiletime
- code completion 
- refactoring
- shorthand notations

---
## # TypeScript Drawbacks

- Compilation - "Transpilation" - browser translates .ts into .js
- Discipline in coding

---
## # Additional Types

There are some additional types, that vanialla JS does not support.
Look at [[00_TS_DataTypes]].

| JavaScript | TypeScript |
| ---------- | ---------- |
| number     | any        |
| string     | unknown    |
| boolean    | never      |
| null       | enum       |
| undefined  | tuple      |
| object     |            |

Note: in TypeScript, you don't even need to define a variable with its type.

```ts
// The compiler already knows, that it's a number
let sales = 123;
// The compiler says the same with this one
let sales2: number = 234;
```

---