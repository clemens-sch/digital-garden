#Devices 

---
## # What is Debouncing?

It can be challenging to connect buttons or switches to microcontroller inputs
- the electric signal can bounce - looks like you pressed it several times quickly
- electrical contacts make & break contact multiple times "contact bounce" --> leads to rapidly changing signals - can cause problems
- input voltage toggles a few times: HIGH --> LOW --> HIGH --> ...

With `Debouncing` chattering _(unwanted signals)_ gets eliminated
- So the system recognizes a single press - not many
- It ensures a stable & safe input
---
### # Example of chattering
for input signals on/off

![[Pasted image 20231108182338.png]]

`Debounce` can be done in hardware or software
There exists many debouncing methods

---
### # Hardware Debouncing
- Bounce-Free Switches
	- special & expensive

![[Pasted image 20231108182733.png]]

- Debouncing ICs (Integrated Circuits)

#### # Schmitt Trigger
Is like a traffic light for electrical signal

- changes state through "stop" & "go"
- it's like a decision maker that waits for a clear choice before changing its mind

RC filter followed by a Schmitt Trigger:

![[Pasted image 20231108183021.png]]

#### # Cross-coupled NAND debounce

![[Pasted image 20231108183349.png]]

---
### # Software Debouncing

Delay at reading inputs 

![[Pasted image 20231108183702.png]]

there also exists a counter btw
but nvm how to use it haha
