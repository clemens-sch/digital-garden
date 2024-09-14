#Software-Engineering 

---
## # Func<> 

A Func<> is a generic delegate.
A Func has 0 to 16 input-parameters. It always has an output parameter.

This type of delegate can reference a method, that returns a type.

---
### # Declaration 

```csharp
// This Func only takes a string as input and output type
//   input,  output
Func<string, string> pointer = SomeMethod;

string SomeMethod(string str)
{ }
```


If a Method has a Func<> in its signature, you can pass in another method in the signature. 

```csharp
SomeOtherMethod(Func<string, string> Formater)
{ }
```

---
## # Action<>

Takes a method, that performs any action.
But it does NOT return any type. 

---
## # Predicate<>

Points to methods, that take one parameter of any type. 
It always returns a boolean. 

----