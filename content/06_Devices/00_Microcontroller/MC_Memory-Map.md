#Devices 

---
#### # Data memory map of ATmega328p

![[Pasted image 20231106182215.png]]

- 32 general purpose working registers (GPR)
	- temporary storage; doing calculations
- 64 I/O registers ([[MC_Special-Function-Register]], I/O memory)
	- special registers for controlling & setting things up
	- the functions:
		- Status register
		- Timer
		- Serial communication
		- I/O Ports
		- ...
- 160 extended I/O registers
	- extra registers for more tasks
	- the functions:
		- I/O Ports
		- ...
- 2048 (ATmega328p) Bytes of internal data SRAM (Static Random Access Memory)
	- a chunk of memory - storing data temporarily 
	- directly accessible
	- 8 bits wide

