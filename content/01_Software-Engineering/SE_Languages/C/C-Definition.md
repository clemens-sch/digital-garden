#Software-Engineering 

---
## # What is C?

C is a general-purpose programming language created by Dennis Ritchie at the Bell Laboratories in 1972.

It is a very popular language, despite being old. The main reason for its popularity is because it is a fundamental language in the field of computer science.

C is strongly associated with UNIX, as it was developed to write the UNIX operating system.

---
## # Why Learn C?

- one of the most popular programming language in the world
- similar syntax to other programming languages (Java, Python, C++, C#, ...)
- C is very fast, compared to other programming languages, like [Java](https://www.w3schools.com/java/default.asp) and [Python](https://www.w3schools.com/python/default.asp)
- C is very versatile; it can be used in both applications and technologies
---
## # Difference C vs C++

- [C++](https://www.w3schools.com/cpp/default.asp) was developed as an extension of C - both have similar syntax
- The main difference: C++ support classes and objects, while C doesn't
---
## # simple C programm

```c
#include <stdio.h>
int main int argc , char ** argv
{
	// print a message in the terminal
	printf ("Main Program Entry point \n");
	// returns an integer 
	return 0;
}

// Main function without parameters
int main(void) { ... }
```
---
## # Include libraries

with the `#include ...` syntax

* <> ..       include standard system libraries
	* <`stdio.h`>
* "" ..         include local library from path
	* "myLib.h"

---
### # Header files

you can create own librariese or split functionality

Usage:
- global defines (not for global variables!)
- reusable software

myLib.h:

```c
#ifndef __MYLIB_   // Check if the macro __MYLIB_ is not defined
#define __MYLIB_   // Define the macro __MYLIB_

/* Function prototype for a global function */
int mylibfnc(int param);

#endif              // End of the conditional compilation block
```

myLib.c:

```c
#include <stdio.h>
// include Header file
#include "myLib.h"

// global function
int mylibfnc(int param)
{
	printf("Doing some library stuff... param= %d \n", param);
	return 0;
}
```
---
#### # Global Variables
- allowed
- not nice!
- memory reserved at runtime
- do **not** use Header files for global vars
- define global vars only once!

Access from all files

```c
#include <stdio.h>
int globVar1 = 3;      // global variable
extern int globVar2;   // var is defined somewhere else
#include "myLib.h"

// global function
int mylibfnc(int param)
{
	printf("Doing some library stuff... param=%d \n", param);
	return 0;
}
```
---

