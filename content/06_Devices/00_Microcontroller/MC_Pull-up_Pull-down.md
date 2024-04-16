#Devices 

---
## # What are pull-up/pull-down resistors

- components used in electronic circuits
- ensure a digital input signal has a defined voltage level : 1(HIGH)/0(LOW)
	- when no other active source influences the signal
- avoids disturbances, noise, mistakes, ...
---
#### # Pin Input Circuit using a button or switch
How to connect a switch to a digital input pin?

![[Pasted image 20231108173607.png]]

prevent a floating input voltage
- no disturbances, noise, ...
---
## # External Resistors

physical components - not built in the microcontroller

### # External Pull-down Resistor
connected between digital input & positive supply voltage (VCC/5V)

- puts voltage to LOW = 0

![[Pasted image 20231108174842.png]]

Button closed - HIGH input
Button open - LOW input

---
### # External Pull-up Resistor
connected between digital input & Ground (GND)

- puts voltage to HIGH = 1

![[Pasted image 20231108175029.png]]

Button closed - LOW input
Button open - HIGH input

---
## # Internal Resistor

Built into the microcontroller for digital IC
Typically controlled through software settings
- not physical components
### # Internal Pull-up Resistor

- ATmega has internal pull-up resistors
- Configuration with
	- PORTx
	- DDRx

![[Pasted image 20231108180223.png]]

Button closed: input is LOW
Button open: input is HIGH


