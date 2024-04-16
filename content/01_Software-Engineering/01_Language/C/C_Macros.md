#Software-Engineering 

---
## # What are Macros?

A Macro is a code fragment with name (UPPERCASE normally).
It is a shorthand to define longer pieces of code.

Usage: A preprocessor replaces _name => code fragment_

---
### # define vs undef

`#define` - create macro with name (maybe value)
- create code snippets, constants, enable conditional compilation

`#undef` - remove definition of macro
- reset include guards 
---
### # simple example: .h - impl.c - main.c

Here is an example with a header file and a main program file.
In this example, we define a function in the header file - we use this function in the main program.

example.h
```c
#ifndef EXAMPLE_H  // Include guards to prevent multiple inclusion
#define EXAMPLE_H  // macro: creates EXAMPLE_H symbol - to only be used once

// Function declaration
int addNumbers(int a, int b);

#endif  // EXAMPLE_H
```

example.c
```c
#include "example.h"

// Function definition
int addNumbers(int a, int b) {
    return a + b;
}
```

main.c
```c
#include <stdio.h>
#include "example.h"  // Include the header file

int main() {
    // Using the function from the header file
    int result = addNumbers(5, 3);

    // Display the result
    printf("Result: %d\n", result);

    return 0;
}
```
---
### # Object-like macro

example.h
```c
#ifndef EXAMPLE_H
#define EXAMPLE_H

#define MAX 100

// function, which uses MAX
int limitValue(int value);

#endif  // EXAMPLE_H
```

example.c
```c
#include "example.h"

int limitValue(int value) {
    // MAX-define - ensures that value < 100 (MAX)
    return (value > MAX) ? MAX : value;
}
```

main.c
```c
#include <stdio.h>
#include "example.h"

int main() {
    int x = 120;

    // Function, that implements MAX
    int result = limitValue(x);
    // result = 100 - 120 gets limited to 100

    printf("Limited value: %d\n", result);

    return 0;
}
```

### # Function-like macro

example.h
```c
#ifndef EXAMPLE_H
#define EXAMPLE_H

#define INCREMENT(x) ++x
#define SUM(a, b) a + b

#endif  // EXAMPLE_H
```

main.c
```c
#include <stdio.h>
#include "example.h"

int main() {
    int x = 120;

    // Verwendung des INCREMENT-Defines
    INCREMENT(x);

    // Verwendung des SUM-Defines
    int sumResult = SUM(3, 5);

    printf("Incremented value: %d\n", x);
    printf("Sum result: %d\n", sumResult);

    return 0;
}
```