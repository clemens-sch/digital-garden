#School 

---
- Timer
- LCD (library: LCD-Driver auf Brunner-Website)
	- lcd_definitions setzen
- Schaltplan zeichnen
	- Ports zeichnen & beschriften
- Ultraschall-Sensor
	- Default Schwellwert: 0,5m; Taste bei Drücken immer + 0,5 - bis zu 5m 
		- wenn 5m, dann wieder 0,5m
	- Schwellwert einstellen von 3: LED soll leuchten; darunter: LED erlischt
- LED
	- Temperator == Entfernung

---
Teams-Aufgabe

Ultraschallsensor - 0-5V
0 - 1023

ADC retourniert Ganzzahl --> wir wandeln Int in String um
- SYTI4 > ADC > Bsp. f. itoa

sprintf() = konstanter Wert + Value
itoa(errechnerter Ultraschallsensor-wert, buffer, 10);
- 512 = ASCII - 51 49 50

alle 2000 Millisekunden messen (vlt timer/delay)
- wert über USART übertragen
- abgabe: main.c