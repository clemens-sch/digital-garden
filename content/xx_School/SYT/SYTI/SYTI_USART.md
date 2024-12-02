#school 

---
## # USART 1

"Universal Synchronous and Asynchronous serial Receiver and Transmitter"
- digitale Schnittstelle, die serielle Datenübertragung ermöglicht

> ATmega328P nur EIN USART (USART0)

Anwendungsbereiche:
- Übertragen von Werten
- Mensch-Maschine-Schnittstelle

---
## # Datenleitungen

Die asynchrone Variante bietet naben GND weitere Datenleitungen:
- TxD - Transceive Data - Daten senden
- RxD - Receive Data - Daten empfangen
---
## # Benötigte Systemeinstellungen

1. Geschwindigkeit (Baud-Rate)
2. Anzahl d. Datenbits (i. d. R. 8)
3. Stop-Bit (Ja/Nein)
4. Parity (Ja/Nein)
5. Flow Control

---

![[Pasted image 20240920124839.png]]

---
### # Baud-Rate

Geschwindigkeit der Kommunikation zw. Sender und Empfänger - Baud-Rate muss auf beiden Seiten gleich sein.

