#Software-Engineering 

---
## # What are Header files?

Header files contain C declarations, [[C_Macros]] definitions & functions.
File extension: `.h`.

Are shared between several source files - allow you to declare functions, [[C_Macros]], ... in one place & you can use them across different parts of your programm.

---
### # Why use header files

- reusability
- seperate declaration from implementation 
- encapsulation
- conditional choices
---
### # Include header files
using the `include` directive

```c
#include <unistd.h>          // unistd.h - used for system calls
#include "mylibs/mylib.h"    // own header files
```

The `#include` directive copies the content of header file into source file - during preprocessing stage.
Shows compiler, that declarations are available when processing each source file.

---
### # Search path

`" "` - compiler first looks for header file in same directory as source file
	not found - checks list of standard system directories.

`< >` - compiler only looks in standard system directories.

---
### # Conditinal compilation directives

To avoid errors/conflicts by multiple inclusions of same header file
- use conditional compilation directives

```c
#ifndef FILE_FOO_SEEN 
#define FILE_FOO_SEEN 

/* the entire header file */ 

#endif /* !FILE_FOO_SEEN */
```

_How this works:_
- If FILE_FOO_SEEN has not been defined before (first time header is included) - gets defined & header is included
- If FILE_FOO_SEEN has been defined (header has already been included) - content in `#ifndef` to `#endif` is skipped

#### # sample keywords

`#ifndef` (if not defined) - checks, whether a macro has been defined.
- is not - code between `#ifndef` to `#endif` is included
- is - code is skipped
  
`#define` - defines macro.

`#endif` - marks end of conditional block.

---