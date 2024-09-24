#Definitions 

---
## # USART ?

`Universal Synchronous and Asynchronous serial Receiver and Transmitter`

2 Betriebsarten: synchron, <mark style="background: #D2B3FFA6;">asynchron</mark> (wir verwenden asynchron)

Realisiert digitale Schnittstelle
- ermöglich serielle Datenübertragung
- für längere Distanzen!
- läuft über Port D (PD0, PD1)
- Geschwindigkeit = Baud-rate (Anzahl der Symbole/Zeiteinheiten)

Bsp.: Flashen, Übertragen v. Werten, Mensch-Maschine-Schnittstelle

---
## # Blockschaltbild

skizze: siehe Samsung Notes

wir brauchen:
- TxDn
- RxDn

3 Config-Register + Status-Info:
- UCSRnA
- UCSRnB
- UCRSnC

UDR0 - 1 Register für Senden & Empfangen eines Bytes!

- UBRRn
- UDRn

wir brauchen nicht:
- Clock - XCKn

![[Pasted image 20240920124839.png]]

![[Pasted image 20240920080545.png]]

---
## # Datenblatt Baud-rate

je nach Leitungslenge - spezifische Baud-rate wählen.

standard: im Datenblatt: 9600 Baud = 103 im UBRRn

asynchronous normal mode: ... - formel für automatische berechnung

---
## # Andere Interfaces

TWI - "two wired interface"
SPI
- typischer on-Board Bus (Bus - mehrere können angereiht werden)


