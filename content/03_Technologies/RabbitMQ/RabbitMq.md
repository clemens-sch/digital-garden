#Technologies 

---
## # REST Problem ?!

Beide Seiten müssen zur gleichen Zeit erreichbar sein.

![[Pasted image 20241203081754.png]]

---
## # Messagebroker

Haben nur eine Aufgabe.
Man geht davon aus, dass diese sehr verlässlich & ausfallsicher sind.
**Entkoppelung** zw. Producer ↔ Consumer

![[Pasted image 20241203081740.png]]

In diesem Beispiel ist die Kommunikation zw. Producer ↔ Consumer entkoppelt.
- Einzige Point-Of-Failure ist von Producer → Messagebroker
- Wir gehen davon aus, dass die Nachricht gesendet wird und irgendwann beim Consumer ankommt 

---
### # MOM System

"Message Oriented Middleware"
- asynchron - "Fire and forget" - Verschicken der Nachricht blockiert Sender nicht

Bei Ausfall von Kommunikationspartner - wenn beide parallel online sind, Consumer holt sich Nachricht ab (komplett Entkoppelt)

---
### # Kommunikation

Unterschied: Queue vs. Topic

Queue (eine eindeutige Nachricht nur ein einen einzigen Consumer zugestellt):
- Producer ↔ Consumer

Topic (eine eindeutige Nachricht wird vervielfältigt an alle Subscriber):
- Publisher ↔ Subscriber

---
## # MOM Protokolle

- STOMP - "Simple Text Oriented Protocol"
- AMQP - "Advanced Message Queue Protocol"
- MQTT

---
### # Exchange vs Queue

Exchange - dort schicken wir Nachricht hin
Queue - dort holen wir Nachrichten ab

---
### # Directexchange

Routingkey = HTTP-Header

wenn Routingkey ... ist, schicke an ...
wenn Routingkey ... ist, schicke an ...

---
### # Topicexchange



---
### # Fanoutexchange

Typischer Broadcast-Fall

wenn Nachricht hereinkommt - schicke Nachricht an alle in der Qeue.

