#school 

---
### # ADMUX

"ADC Multiplexer Selection Register"

1. ADMUX auf Port einstellen
	1. 0101b - Poti
		1. MUX2
		2. MUX0

3. Prescaler setzen
	1. ADEN - Enable
	2. Prescaler v. 128 = ADPS0, ADPS1, ADPS2
		1. Prescaler sagt aus, wie oft der ADC auslesen soll
		2. Prescaler = F_CPU / gewünschte Frequenz (Bsp.: 125kHz) 
		3. Wenn bspweise jede Sekunde lesen soll - 

---
### # Verwendung

"Analog Digital Converter"
Verwendet um elektrische Spannung in Digitalzahl zu konvertieren

Wichtige Register sind:
1. ADMUX
2. ADCSRA
	1. ADATE
3. Bei ADATE = ADTS0 bis ADTS2

---
### # ADMUX

"ADC Multiplexer Selection Register"

- Referenzspannung
- Kanal, an welchem analoges Signal anliegt
	- angenommen: analogen Eingang "Kanal3" = 0011b = Bits MUX0 bis MUX3

---
### # ADCSRA

"ADC-control and status Register"

- ADEN - ADC Enable
- ADSC - ADC Start Conversion
- ADIE - ADC Interrupt Enable
- ADPS - ADC Prescaler Select Bits

---
### # ADC-Statusabfrage + Single-Conversion

```c
ADCSRA |= (1<<ADEN) | (1<<ADPS0) | (1<<ADPS1) | (1<<ADPS2);

while (1)
{
	ADCSRA |= (1<<ADSC);
	// Warten, bis Messung abgeschlossen...
	while (ADCSRA & (1<<ADSC));
	// Messung fertig, Ergebnis auslesen
	tmp = ADCW;
}

// ADC - liefert 10-Bit Wert = 16Bit Variable
volatile uint16_t tmp;
main() {
	// Single-Conversion mit ISR
	// Free Running mit ISR
	ADCSRA |= (1<<ADEN) | (1<<ADIE) | (1<<ADATE) | (1<<ADPS0) | (1<<ADPS1) | (1<<ADPS2);
	ADCSRA |= (1<<ADSC);
	
	while(1)
	{
		// do sth -auf tmp zugreifen (ATOMIC BLOCK verwenden) & weitere Messung starten
		// unser Controller hat 8 Bit Takt - wollen in 16 Bit Variable zuweisen = ATOMIC BLOCK = wird nicht unterbrochen
		if()
	}
}

// ISR - so kurz wie möglich halten!
ISR(ADC_vect){
	tmp = ADCW;
}
```
