#school 

---

## # Verwendung mit Treiber

dht.h:

```c
//setup port: Sensor an Port D, Pin 2 anschließen
#warning define your Pins here!
#define DHT_DDR DDRD
#define DHT_PORT PORTD
#define DHT_PIN PIND
#define DHT_INPUTPIN 2

//sensor type
#define DHT_DHT11 1
#define DHT_DHT22 2
// Unserer ist DHT11 - diese Konstante DHT_DHT11
#define DHT_TYPE DHT_DHT11

//enable decimal precision (float)
#if DHT_TYPE == DHT_DHT11
#define DHT_FLOAT 0
#elif DHT_TYPE == DHT_DHT22
#define DHT_FLOAT 1
#endif

//timeout retries - nicht weniger als 200ms! Zugriffszyklus muss >= 200ms sein!
#define DHT_TIMEOUT 200

//functions
#if DHT_FLOAT == 1
//extern int8_t dht_gettemperature(float *temperature);
//extern int8_t dht_gethumidity(float *humidity);
//extern int8_t dht_gettemperaturehumidity(float *temperature, float *humidity);
#elif DHT_FLOAT == 0
// Nur das ist bei uns verfügbar!
extern int8_t dht_gettemperature(int8_t *temperature);
extern int8_t dht_gethumidity(int8_t *humidity);
extern int8_t dht_gettemperaturehumidity(int8_t *temperature, int8_t *humidity);
#endif
```
