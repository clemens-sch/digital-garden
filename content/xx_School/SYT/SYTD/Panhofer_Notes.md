#Definitions 

---
## # Services

Service = Software-Komponente, die in eigenem Betriebssystemprozess ausgeführt wird.

- SOA-Architektur
	- Netflix - Mikroservicearchitektur
	- Bsp.: Amazon kann keine SOA-Architektur fahren (muss 1mio. Request verarbeiten)
		- Lösung: Message-Broker

![[Pasted image 20241004122757.png]]

---
## # Messagebroker

Nimmt Nachricht entgegen und speichert solange, bis consumer wieder da ist.
- RabbitMQ = Messagebroker

Arbeitet asynchron = Request arbeitet als fire-and-forget.
- Sender wird nicht blockiert

![[Pasted image 20241004123225.png]]

Queue-System: first-in & first-out

---
## # DirecteXchange

exchange

![[Pasted image 20241004124137.png]]

queue:

![[Pasted image 20241004124204.png]]

![[Pasted image 20241004124244.png]]

we have created binding + queue

![[Pasted image 20241004124414.png]]

now, the message is published

....

![[Pasted image 20241004124605.png]]

![[Pasted image 20241004124647.png]]

Get sent messages:

![[Pasted image 20241004124712.png]]

---
## # TopiceXchange

key definieren: eine Nachricht an alle 3
- oder einen rauspicken und an nur einen senden
- SEW-Queue
- INSY-Queue
- SYTG-Queue

Lösung siehe RabbitMQ-Website localhost:15672

---
## # FanounteXchange

![[Pasted image 20241004130234.png]]

