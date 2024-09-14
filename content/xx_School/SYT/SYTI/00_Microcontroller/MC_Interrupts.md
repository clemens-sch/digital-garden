#Devices 

---
## # What are Interrupts?

Signal, that immediatelly stops processor's current task - handles urgent event.
- often triggered by external (peripheral) sources

Interrupts occur independently & asynchrony to running tasks (don't wait for processor, till current task is finished).

An external device makes an IRG (interrupt request), signaling the processor to interrupt its current task & address a pending event.

---
### # Sequential Programming

<mark style="background: #D2B3FFA6;">"Polling"</mark> = permanently checks if something happens
- checks status of inputs, registers, sensors

1. starting PC - starting a programm
2. set things up - initialize variables, prepare environment
`while (1) {`
3. check inputs (_polling_) - checks if some input is made (e.g. pressing a Button)
4. do service - condition is met - performs service
5. set outputs - update state of things
`}`

![[Pasted image 20231213185956.png]]

---
### # traps, exceptions, faults

something unexpected/wrong happened while execution of a program.

There are **External Traps**
- come from outside the program - problem with PCs memory, unable to access something (PCs memory - Bus error), ...

And there are **Internal Traps**
- inside the program/PC itself - divide number by zero, breakpoints, ...
---
### # ISR

`ISR` = Interrupt Service Routine

each interrupt has own `ISR`. 
when an interrupt occurs, `ISR` is executed.

most microcontrollers have fixed memory location with address of ISR = Interrupt Vector Table (`IVT`).
- `IVR` list that tells where to find `ISR`
---

