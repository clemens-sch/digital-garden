#school 

---
# # ADC-USART/LCD-Interrupt-Mode
```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <avr/interrupt.h>
#include <stdio.h>
#include "lcd.h"  // Für LCD-Ausgabe (optional)

volatile uint16_t adc_value = 0;  // Globaler ADC-Wert
volatile uint8_t new_data = 0;    // Flag, um neue Daten zu signalisieren

void USART_init(uint16_t ubrr) {
    UBRR0 = ubrr;                      // Baudrate setzen
    UCSR0B |= (1 << TXEN0);            // Transmitter aktivieren
    UCSR0C |= (1 << UCSZ00) | (1 << UCSZ01);  // 8 Datenbits, 1 Stoppbit
}
void USART_send_char(uint8_t data) {
    while (!(UCSR0A & (1 << UDRE0)));  // Warten, bis das Register leer ist
    UDR0 = data;
}
void USART_send_string(const char *str) {
    while (*str) {
        USART_send_char(*str);
        str++;
    }
}
void ADC_init() {
    ADMUX |= (1 << REFS0) | (1 << MUX2) | (1 << MUX0);  // Referenzspannung und Kanal ADC5 (PC5)
    ADCSRA |= (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0); // Prescaler 128 (125 kHz)
    ADCSRA |= (1 << ADIE) | (1 << ADEN);  // ADC-Interrupt und ADC aktivieren
}
ISR(ADC_vect) {
    adc_value = ADC;  // ADC-Wert speichern
    new_data = 1;     // Flag setzen, um neue Daten zu signalisieren
}
int main(void) {
    char buffer[16];  // Puffer für den formatieren String
    float voltage;
    // USART und ADC initialisieren
    USART_init(103);  // 9600 Baud bei 16 MHz
    ADC_init();
    // LCD initialisieren (falls verwendet)
    lcd_init(LCD_DISP_ON);
    sei();  // Interrupts global aktivieren
    // Start der ersten ADC-Konvertierung
    ADCSRA |= (1 << ADSC);
    while (1) {
        if (new_data) {
            // Spannung berechnen
            voltage = (adc_value * 5.0) / 1023.0;
            // Wert in String umwandeln
            sprintf(buffer, "Volt: %.2fV\n", voltage);
            // USART-Ausgabe
            USART_send_string(buffer);
            // Optional: Ausgabe auf dem LCD
            lcd_clrscr();
            lcd_puts(buffer);
            new_data = 0;  // Flag zurücksetzen
            // Nächste ADC-Konvertierung starten
            ADCSRA |= (1 << ADSC);
        }
    }
    return 0;
}
```

---
# # ADC-USART/LCD-Polling-Mode
```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>
#include <stdio.h> 

uint16_t ADC_read() {
	// Start conversion
	ADCSRA |= (1 << ADSC);
	// warten bis Konvertierung fertig ist
	while (ADCSRA & (1 << ADSC));
	// ADC-Wert
	return ADC;
}
void USART_send_char(uint8_t data) {
	// Warten, bis das UDR0-Register leer ist
	while (!(UCSR0A & (1 << UDRE0)));
	UDR0 = data;
}
void USART_send_string(uint8_t *str) {
	while (*str) {
		USART_send_char(*str);
		str++;
	}
}
int main(void) {
	// USART konfigurieren: 9600 Baud, 8 Datenbits, 1 Stoppbit
	UBRR0 = 103;
	UCSR0B |= (1 << TXEN0);
	UCSR0C |= (1 << UCSZ00) | (1 << UCSZ01);
	
	// ADC auf analoge Eingaben von PORTC5 hören lassen
	ADMUX |= (1 << REFS0) | (1 << MUX2) | (1 << MUX0);
	// ADC prescaler auf 128 (16MHz Taktfreq., ADC Takt auf 125kHz)
	ADCSRA |= (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0) | (1 << ADEN);
	while (1) {
		// ADC-Wert einlesen
		uint16_t adc_value = ADC_read();
		
		// Spannung berechnen
		float voltage = (adc_value * 5.0) / 1023.0;
		
		// Float-Wert in String umwandeln
		// Floating-Point Error "?"
		uint8_t buffer[16];
		sprintf(buffer, "Volt: %.2fV\n ", voltage);
		// String über USART senden
		USART_send_string(buffer);
		_delay_ms(1000);
	}
	return 0;
}
```

![[Pasted image 20241128204303.png]]

---
# # Digital I/O
```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>
#include <stdio.h>
#include "lcd.h"

int main(void)
{
	// PB0 als Ausgang setzen
	DDRB |= (1 << DDB0);
	// Pull-up Widerstand von Button - bleibt HIGH, solange nicht gedrückt wird
	PORTB |= (1 << PORTB1);

    while (1) 
    {				
		// PINB - aktuellen Zustand von Port B lesen
		// wenn PINB == 0, LED leuchtet; PINB & (1 << PINB1) == 1; 
		if (!(PINB & (1 << PINB1)))
		{
			PORTB |= (1 << PORTB0);
		} 
		else 
		{ 
			//PORTB = PORTB & ~(1 << PORTB0);
			PORTB &= ~(1 << PORTB0); 
		}
		_delay_ms(100);
    }
	return 0;
}

```

---
# # LCD
![[Pasted image 20241128205015.png]]

---
# # Timer
```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <avr/interrupt.h>
#include <stdio.h>
#include "lcd.h"

// Globale Variable zum Speichern des ADC-Werts
volatile uint16_t adc_value = 0;
// ADC-Interrupt Service Routine
ISR(ADC_vect) {
    adc_value = ADC; // Speichere den ADC-Wert in der globalen Variable
}
// Timer0 Overflow Interrupt Service Routine
ISR(TIMER0_OVF_vect) {
    // Der Timer löst automatisch den ADC-Start aus, kein manuelles Starten nötig.
}

void ADC_init(void) {
    // ADC konfigurieren: AVcc als Referenz, Kanal ADC5 (PC5)
    ADMUX |= (1 << REFS0) | (1 << MUX2) | (1 << MUX0);
    // ADC aktivieren, Auto Trigger Modus, Interrupt erlauben, Prescaler auf 128
    ADCSRA |= (1 << ADEN) | (1 << ADATE) | (1 << ADIE) | (1 << ADPS2) | (1 << ADPS1) | (1 << ADPS0);
    // Trigger-Quelle: Timer0 Overflow
    ADCSRB = (0 << ADTS2) | (0 << ADTS1) | (1 << ADTS0);
    // Erste Konvertierung starten
    ADCSRA |= (1 << ADSC);
}
void Timer0_init(void) {
    // Timer0 konfigurieren: Normal Mode, Prescaler 1024
    TCCR0A = 0x00; // Normal Mode
    TCCR0B |= (1 << CS02) | (0 << CS01) | (1 << CS00); // Prescaler auf 1024
    
    // Timer Overflow Interrupt aktivieren
    TIMSK0 |= (1 << TOIE0);
}
int main(void) {
    // LCD initialisieren
    lcd_init(LCD_DISP_ON);
    lcd_clrscr();
    // ADC und Timer0 initialisieren
    ADC_init();
    Timer0_init();

    // Globale Interrupts aktivieren
    sei();
    char buffer[16];
    while (1) {
        // ADC-Wert auf LCD ausgeben
        float voltage = (adc_value * 5.0) / 1023.0;
        sprintf(buffer, "Volt: %.2fV", voltage);
        lcd_clrscr();
        lcd_puts(buffer);
        _delay_ms(500);
    }
}

```