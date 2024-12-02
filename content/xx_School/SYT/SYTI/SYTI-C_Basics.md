#school 

---
### # Input/Output

Input = liest Signale v. externen Geräten (Sensor, Schalter, ...)
> Externes Signal --> Mikrocontroller
- Bsp.: Prüfen, ob Schalter gedrückt ist
- Bsp.: Messen von Sensorwerten
Output = sendet Signale an externe Geräte (LEDs, Motoren, ...)
> Mikrocontroller --> Externes Gerät
- Bsp.: Einschalten v. LED
- Bsp.: Steuern v. Motor

---
### # Digital/Analog

Digital = zwei Zustände
- HIGH - 5V/3.3V
- LOW - 0V
einfache Zustandsabfragen - Button gedrückt oder nicht? LED eingeschaltet oder nicht?

Analog = kontinuierliche Spannungswerte zw. 0V und 5V
- durch ADC in Zahlen konvertiert
Messung von Signalen - Sensor-Daten; ...

---
### # Pull-up/Pull-down

Definieren immer klaren Spannungszustand: HIGH/LOW.
- Verhindert, dass Pin ungewollte Signale sendet/liest.

---
### # DDRx/PORTx/PINx

DDRx - erlaubt Einschalten v. Bits in spezifischen Register
PORTx - Setzen v. Ausgangsspannung auf HIGH/LOW oder Pull-up Widerstände
PINx - Liest aktuellen Zustand von Pin: HIGH/LOW

---
### # Bitmasks

| - logisches Oder 
& - logisches Und
~ - Invertieren aller Bits

Gibt auch Kurzschreibweise:

```c
{
...
DDRB |= (...)
PORTB |= (...)
PORTB &= (...)
}
```

---
### # Interrupts

Verhindert "Polling" (ständiges Abfragen)
Unterbricht normale Programmausführung - füht ISR aus - kehrt zur normalen Ausführung zurück.

