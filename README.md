# temperatura

## – DHT11 + LEDs

## 📦 Materiais necessários
- 1x ESP32
- 1x Sensor de temperatura e umidade DHT11
- 1x LED azul
- 1x LED vermelho
- 2x Resistores (220 Ω ou 330 Ω)
- Jumpers
- Protoboard

## 🔌 Ligações
### DHT11
- Pino DATA → Pino 2 do ESP32
- Pino VCC → 3.3V
- Pino GND → GND

## LEDs
- LED azul → Pino 4
- LED vermelho → Pino 18  
  (cada LED com resistor em série)



## 📜 Código
```cpp
/*
  Read Temperature and Humidity
  DHT11 Library
  Author: Bonezegei (Jofel Batutay)
  Date : November 2023

  Tested using ESP32-WROOM32
*/

#include <Bonezegei_DHT11.h>

// param = DHT11 signal pin
Bonezegei_DHT11 dht(2);

// Definição dos pinos dos LEDs
#define LED_AZUL 4
#define LED_VERMELHO 18

// Definição do limite de temperatura
#define LIMITE_TEMP 25.0

void setup() {
  Serial.begin(115200);
  dht.begin();

  pinMode(LED_AZUL, OUTPUT);
  pinMode(LED_VERMELHO, OUTPUT);

  // Apaga os LEDs no início
  digitalWrite(LED_AZUL, LOW);
  digitalWrite(LED_VERMELHO, LOW);
}

void loop() {
  if (dht.getData()) {                         // get All data from DHT11
    float tempDeg = dht.getTemperature();      // return temperature in celsius
    float tempFar = dht.getTemperature(true);  // return temperature in fahrenheit
    int hum = dht.getHumidity();               // return humidity

    String str  = "Temperature: ";
           str += tempDeg;
           str += "°C  ";
           str += tempFar;
           str += "°F  Humidity:";
           str += hum;
    Serial.println(str.c_str());

    // Controle dos LEDs conforme a temperatura
    if (tempDeg < LIMITE_TEMP) {
      digitalWrite(LED_AZUL, HIGH);
      digitalWrite(LED_VERMELHO, LOW);
    } else {
      digitalWrite(LED_AZUL, LOW);
      digitalWrite(LED_VERMELHO, HIGH);
    }
  }

  delay(2000);  // delay mínimo de 2 segundos para DHT11 ler os dados
}


## Explicação do funcionamento
O ESP32 lê os dados de temperatura (°C e °F) e umidade (%) pelo DHT11.

Os valores são exibidos no Serial Monitor a cada 2 segundos.

Se a temperatura estiver abaixo de 25 °C, o LED azul acende.

Se a temperatura for igual ou superior a 25 °C, o LED vermelho acende.
