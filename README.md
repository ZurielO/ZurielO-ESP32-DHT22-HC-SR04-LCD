# ZurielO-ESP32-DHT22-HC-SR04-LCD
Uso de ESP32 con Sensor DHT22, HC-SR04 y Pantalla LCD

## Objetivo 
El objetivo de esta práctica es utilizar la ESP32 junto con el sensor de temperatura y humedad DHT22, el sensor de distancia HC-SR04 y una pantalla LCD para medir y visualizar en tiempo real la temperatura, humedad y distancia. La práctica permite la integración de múltiples sensores con la placa ESP32 para la creación de un sistema interactivo y funcional.

## Material 
- Placa ESP32: Microcontrolador con capacidades de Wi-Fi y Bluetooth, ideal para aplicaciones IoT.
- Sensor DHT22: Sensor digital para medir temperatura y humedad con alta precisión.
- Sensor HC-SR04: Sensor de ultrasonido para medir distancias en centímetros.
- Pantalla LCD 20x4 con I2C: Pantalla para visualizar los datos de los sensores.
- Plataforma Wokwi: Plataforma de simulación electrónica en línea para el diseño y prueba de circuitos sin necesidad de hardware físico.

## Diagrama de conexión
Sensor DHT22: El pin de señal del sensor está conectado al GPIO 15 de la ESP32.
Sensor HC-SR04:
El pin Trigger está conectado al GPIO 4 de la ESP32.
El pin Echo está conectado al GPIO 2 de la ESP32.
Pantalla LCD 20x4: La pantalla LCD está conectada utilizando el protocolo I2C a la ESP32, con la dirección I2C configurada a 0x27.
Descripción del código
El código está diseñado para leer datos de los sensores DHT22 y HC-SR04, y mostrar la información en un monitor serie y en la pantalla LCD. El flujo de la ejecución es el siguiente:

Configuración del sensor DHT22: Se inicializa el sensor DHT22 en el pin GPIO 15.
Configuración del sensor HC-SR04: Se configuran los pines de Trigger y Echo para medir la distancia mediante el sensor de ultrasonido.
Pantalla LCD: Se utiliza la pantalla LCD para mostrar información sobre la temperatura, humedad y distancia, alternando entre ellas.
Monitor Serie: Los datos también se envían al monitor serie para su visualización en la computadora.

## Código

const int Trigger = 4;   //Pin digital 4 para el Trigger del sensor
const int Echo = 2;   //Pin digital 2 para el Echo del sensor

#include <LiquidCrystal_I2C.h> //Libreria de LCD
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
#include "DHTesp.h" //Libreria de DHT

const int DHT_PIN = 15; //pin del sensor de temperatura
DHTesp dhtSensor;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0

}

void loop() {

  long t; //tiempo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm

  TempAndHumidity data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  Serial.println("---");
  delay(2000);          //Hacemos una pausa de 200ms

  lcd.clear(); 
  lcd.setCursor(4, 0); //coordenadas del LCD
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
  delay(2000);

  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("diego bahena");
  lcd.setCursor(6, 1);
  lcd.print("I.E.E");
  delay(2000);

  lcd.clear(); 
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF" + "C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia: " + String(d) + "cm");
  delay(2000);
}

## Explicación del código
Sensor de distancia HC-SR04:

El sensor HC-SR04 utiliza dos pines: el Trigger para enviar una señal ultrasónica y el Echo para recibirla. El tiempo que tarda el eco en regresar es utilizado para calcular la distancia.
Sensor DHT22:

Se configura para leer la temperatura y la humedad cada vez que se ejecuta el ciclo loop(). Los datos se imprimen en el monitor serie y en la pantalla LCD.
Pantalla LCD:

La información de temperatura, humedad y distancia se muestra secuencialmente en la pantalla LCD cada 2 segundos. Se utilizan las funciones lcd.setCursor() y lcd.print() para posicionar y mostrar los datos en la pantalla.

Monitor Serie:
Los valores de temperatura, humedad y distancia también se envían al monitor serie para su visualización.
Resultados esperados
Al ejecutar el código, los resultados esperados son:
En el monitor serie:

(![Texto alternativo](https://github.com/ZurielO/ZurielO-ESP32-DHT22-HC-SR04-LCD/blob/main/imagen_2024-12-15_173225804.png?raw=true).

En la pantalla LCD: La información se actualizará cada 2 segundos, mostrando la temperatura, humedad y distancia alternadamente en la pantalla LCD.

(![Texto alternativo](https://github.com/ZurielO/ZurielO-ESP32-DHT22-HC-SR04-LCD/blob/main/imagen_2024-12-15_173225804.png?raw=true).

(![Texto alternativo](https://github.com/ZurielO/ZurielO-ESP32-DHT22-HC-SR04-LCD/blob/main/imagen_2024-12-15_173203955.png).


(![Texto alternativo](https://github.com/ZurielO/ZurielO-ESP32-DHT22-HC-SR04-LCD/blob/main/imagen_2024-12-15_173225804.png?raw=true).

## Conclusión
Esta práctica muestra cómo integrar varios sensores con la placa ESP32 y utilizar una pantalla LCD para la visualización de los datos en tiempo real. La utilización de los sensores DHT22 para medir temperatura y humedad, junto con el HC-SR04 para medir distancias, ofrece una solución útil para aplicaciones de monitoreo ambiental e IoT. La práctica es una excelente base para proyectos más complejos que requieran monitoreo de múltiples variables.


