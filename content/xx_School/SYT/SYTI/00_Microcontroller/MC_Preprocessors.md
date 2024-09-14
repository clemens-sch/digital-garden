#Devices 

---
## # What is a Preprocessor?

A preprocessor is a tool, that performs transformations on source code before it is compiled/interpreted.
It prepares code for the next stage of processing (which is compilation).

>It removes unnecessary things in source code. 

Preprocessor tasks:
- file inclusion
- macro expansion
- conditional compilation

_replaces inclusion/macros with corresponding code._

---
### # How a preprocessor works

-  In C - preprocessor is <mark style="background: #D2B3FFA6;">part of compilation process</mark> - it <mark style="background: #D2B3FFA6;">handles macros & directives</mark> in source code.
   Macroprocessor - tool that performs textual transformation based on _predefined rules (macros)_.
   Macros are defined using `#define` 

- preprocessor - automatically invoked by C compiler (part of compilation process)
  
- preprocessor - operates on source code - before it is passed to compiler.
  Goal: prepared source code for compilation stages.
  Transformations it performs:
	- inclusion of files (header files)
	- macro expansion
	- conditional compilation
	- ...

**Sequence - compilation of HLL**

![[Pasted image 20231212194640.png]]

Stages explained:
1. programmer writes code in <mark style="background: #D2B3FFA6;">HLL</mark> (e.g. C)
2. <mark style="background: #D2B3FFA6;">preprocessor</mark> handles macros & directives (automatically invoked by C compiler)
3. After preprocessing - still HLL form, but certain modifications done by preprocessor
   = in <mark style="background: #D2B3FFA6;">Pure HLL</mark> 
4. <mark style="background: #D2B3FFA6;">compiler</mark> generates LLL form - can be understood by computer's architecture
   = <mark style="background: #D2B3FFA6;">Relocatable code</mark> (can run on different memory locations)
5. <mark style="background: #D2B3FFA6;">assembler</mark> translates relocatable code into machine/binary code
   = <mark style="background: #D2B3FFA6;">Assembly language</mark>
6. <mark style="background: #D2B3FFA6;">loader/linker</mark> combine object files - load executable program into memory
7. ouput of linker = binary representation of program - ready to be executed by computer
   = <mark style="background: #D2B3FFA6;">Absolute Machine Code / Locatable Code</mark> (located memory address)
---
