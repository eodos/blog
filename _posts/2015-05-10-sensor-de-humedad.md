---
id: 588
title: 'PFG - Sensor de humedad FC-28'
date: 2015-05-10T17:02:55+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=588
permalink: /proyectos/sensor-de-humedad
categories:
  - Proyectos
tags:
  - humedad
  - i2c
  - pfg
  - sensor
  - tierra
---
## Introducción

El sensor de humedad utilizado es el **FC-28**.  Se basa, al igual que el sensor de lluvia, es un divisor de voltaje ajustable mediante un potenciómetro. Según el fabricante:<img class="  wp-image-589 alignright" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity.jpg?resize=304%2C165" alt="humidity" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity.jpg?w=960&ssl=1 960w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity.jpg?resize=300%2C163&ssl=1 300w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity.jpg?resize=900%2C488&ssl=1 900w" sizes="(max-width: 304px) 100vw, 304px" data-recalc-dims="1" />

>   * This is a summary of the moisture sensor can be used to detect soil moisture, when soil water deficiency, the module outputs a high level, whereas output low level. Using this sensor to water the flowers make an automatic device, without people to manage your garden plants.
>   * Sensitivity can be adjusted (figure blue digital potentiometer adjust)
>   * Soil humidity module on the environment humidity the most sensitive, generally used to detect soil humidity
>   * Module in soil moisture can not reach the setting threshold, DO mouth output high level, when soil moisture more than setting threshold value, module D0 output low level
>   * Has fixed bolt hole, convenient installation
>   * Comparator using LM393 chip, work stability
>   * Two small plate interface specifications (3 wires)
>   * VCC external 3.3~5 V
>   * GND external GND
>   * DO small plate digital output interface (0 and 1)
>   * Digital output D0 can be directly connected with the single chip microcomputer, through the single chip microcomputer to detect high and low level, thus to detect soil humidity

Se va a utilizar la salida analógica del sensor, la cual se ha comprobado mediante experimentos que oscila entre 0 (máxima humedad) y 4080 (nada de humedad). Es necesario por tanto realizar un ajuste experimental del valor para obtener la humedad relativa como un valor entre 0 y 100, siendo 100 la máxima humedad relativa que es capaz de captar el sensor.

Este ajuste se hará calculando una recta tal que en el eje de abcisas esté la salida en V del sensor y en el eje de ordenadas el valor de humedad relativa en % que le corresponda. Conocidos dos puntos (0 mV, 100% HR) y (4080 mV, 0% HR) obtenemos:

<p style="text-align: center;">
  <img src='https://s0.wp.com/latex.php?latex=Pendiente%3A+m+%3D+%5Cfrac%7By_1+-+y_0%7D%7Bx_1+-+x_0%7D+%3D+%5Cfrac%7B0+-+100%7D%7B4080+-+0%7D+%3D+-0.0245&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='Pendiente: m = \frac{y_1 - y_0}{x_1 - x_0} = \frac{0 - 100}{4080 - 0} = -0.0245' title='Pendiente: m = \frac{y_1 - y_0}{x_1 - x_0} = \frac{0 - 100}{4080 - 0} = -0.0245' class='latex' />
</p>

<p style="text-align: center;">
  <img src='https://s0.wp.com/latex.php?latex=Recta%3A+y+%3D+y_0+%2B+m+%5Ccdot+%28x+-+x_0%29+%3D+100+-+0.0245+%5Ccdot+x&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='Recta: y = y_0 + m \cdot (x - x_0) = 100 - 0.0245 \cdot x' title='Recta: y = y_0 + m \cdot (x - x_0) = 100 - 0.0245 \cdot x' class='latex' />
</p>

&nbsp;

## Conexión

El sensor dispone de 4 pines, de izquierda a derecha:

  * Pin 1: salida analógica. Es la que utilizaremos, conectada a una entrada analógica de nuestra placa.
  * Pin 2: salida digital. No se usa.
  * Pin 3: alimentación, se escoge que sea a 3.3V.
  * Pin 4: GND.

&nbsp;

Se decide instalar todos los sensores en la placa Texas Instrument Stellaris Launchpad para comprobar su rendimiento.

A partir del diagrama de la placa TI obtenemos los pines de conexionado:

  * Sensor de temperatura: A11
  * Sensor de lluvia: A9
  * Sensor de humedad: A8

&nbsp;

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_bb.png" data-rel="lightbox-0" title=""><img class="  wp-image-597 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_bb.png?resize=851%2C752" alt="humidity_bb" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_bb.png?w=1854&ssl=1 1854w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_bb.png?resize=300%2C265&ssl=1 300w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_bb.png?resize=1024%2C905&ssl=1 1024w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_bb.png?resize=900%2C795&ssl=1 900w" sizes="(max-width: 851px) 100vw, 851px" data-recalc-dims="1" /></a>

&nbsp;

Nota: los sensores no se corresponden con los instalados pero sirven para ver el conexionado. La placa TI Stellaris utilizada es otra versión, pero comparten los pines que se ven en la imagen.

&nbsp;

## Prueba del sensor

<pre class="lang:arduino decode:true ">#define HUMIDITY_SENSOR A8

void setup() {
  Serial.begin(9600);
}

void loop() {
  int humidity;
  
  humidity = 100-0.0245*analogRead(HUMIDITY_SENSOR);
  
  Serial.println(humidity);
  delay(500);
}
</pre>

El sensor funciona correctamente. Realizando pruebas con una maceta obtenemos valores de 0 - 2% HR cuando la tierra está totalmente seca y valores en torno al 70% HR al regarla. Se observa también como el valor va disminuyendo como a poco al ir secándose la tierra.

&nbsp;

## Comunicación I2C del valor

Se modifican los programas escritos para el sensor de lluvia.

&nbsp;

### Texas Instrument

Como necesitamos conocer el número de bytes que se van a enviar y la humedad oscila entre 0 y 100%, se completan con ceros a la izquierda antes de ser enviado, quedando los valores entre 000 y 100 y enviándose siempre 3 bytes.

<pre class="lang:arduino decode:true ">#include <Wire.h>
#include <math.h>
 
#define SLAVE_ADDRESS 0x08
#define TAG_TEMP "TEMP"
#define TAG_RAIN "RAIN"
#define TAG_HUM "HUMIDITY"
#define TEMPERATURE_SENSOR A11
#define RAIN_SENSOR A9
#define HUMIDITY_SENSOR A8

String var;
char c;

double temperature;
int int_temperature;
int rain;
int humidity;

String value;
char char_array[] = "";
 
// configuracion inicial
void setup() {
  // configuracion de los pines digitales
  pinMode(RAIN_SENSOR, INPUT);
  
  // inicializamos comunicacion serial
  //Serial.begin(9600);
  
  // inicializamos i2c como esclavo
  Wire.begin(SLAVE_ADDRESS);
  
  // llamadas para la comunicacion
  Wire.onReceive(receiveData);
  Wire.onRequest(sendData);
}
 
// bucle principal
void loop() {}

// llamada para recibir datos
void receiveData(int byteCount) {
  
  var = "";
  
  while (Wire.available() > 0) {
    c = Wire.read();
    var.concat(c);
  }
  
  //Serial.print("Received: ");
  //Serial.println(var);
 
  if (var == TAG_TEMP) {
    //Serial.println("TEMP request received");
    temperature = Thermister(analogRead(TEMPERATURE_SENSOR)) *100;
    int_temperature = (int) temperature; 
    value = (String) int_temperature;
  }
  
  else if (var == TAG_RAIN) {
    rain = digitalRead(RAIN_SENSOR);
    if (rain == 1)
      rain = 0;
    else if (rain == 0)
      rain = 1;
    value = (String) rain;   
  }
  
  else if (var == TAG_HUM) {
    humidity = 100-0.0245*analogRead(HUMIDITY_SENSOR);
    value = (String) humidity;
    if (humidity < 10)
      value = String("00" + value);
    else if (humidity <100)
      value = String("0" + value);
  }
  
  // Convert String to char*
  unsigned int value_length = value.length()+1;
  ((String)value).toCharArray(char_array, value_length);
  
}

// llamada para enviar datos
void sendData() {
  Wire.write(char_array);
  //Serial.print("sent: ");
  //Serial.println(char_array);
}
 
// A partir de la salida del termistor calculamos la temperatura
double Thermister(int RawADC) {
  double res;
  double Vo;
  
  Vo = (RawADC)*(3.3/4095.0);
  res = log(10000*(3.3/Vo-1));
  res = 1 / (0.001129148 + (0.000234125 + (0.0000000876741 * res * res ))* res );
  //res = 1 / (0.001126068758413 + ( 0.000234572594529 + (0.000000086312248 * res * res ))* res );
  res = res - 273.15;// Convert Kelvin to Celcius
  return res;
}</pre>

&nbsp;

### Raspberry Pi

Se añade la lectura del sensor de humedad, la inclusión de este valor en el array de valores y la escritura en el fichero LOG.

<pre class="lang:c decode:true ">#include <stdio.h>
#include <stdlib.h>
#include "functions.h"

#define ARDUINO 0x04
#define TI 0x08

#define TAG_REFRESH_RATE "REFRESH_RATE"
#define TAG_TIMESTAMP "TIMESTAMP"
#define TAG_TEMP "TEMP"
#define TAG_RAIN "RAIN"
#define TAG_HUM "HUMIDITY"

int main() {
    char * buf;

    int refresh_rate;

    char * timestamp;

    uint nBytesTAG_TEMP = sizeof(TAG_TEMP)-1; // no enviamos el caracter '\0'
    uint nBytesTAG_RAIN = sizeof(TAG_RAIN)-1;
    uint nBytesTAG_HUM = sizeof(TAG_HUM)-1;
    uint nBytesTEMP = 4;
    uint nBytesRAIN = 1;
    uint nBytesHUM = 3;

    float temperature;
    uint rain;
    uint humidity;

    char temperature_char[] = {0};
    char rain_char[] = {0};
    char humidity_char[] = {0};

    buf = readVar(TAG_REFRESH_RATE);
    refresh_rate = atoi((char *) &buf);
    printf("%s: %d\n", TAG_REFRESH_RATE, refresh_rate);

    for(;;) {
        
        // Leemos los valores se los sensores
        temperature = readSensor(TI, TAG_TEMP, nBytesTAG_TEMP, nBytesTEMP) / 100.00;
        usleep(50000);
        rain = readSensor(TI, TAG_RAIN, nBytesTAG_RAIN, nBytesRAIN);
        usleep(50000);
        humidity = readSensor(TI, TAG_HUM, nBytesTAG_HUM, nBytesHUM);

        //weatherRequest();

        timestamp = getTime();

        // Convertimos los valores a char*
        sprintf(temperature_char, "%.2f", temperature);
        sprintf(rain_char, "%d", rain);
        sprintf(humidity_char, "%d", humidity);

        // Creamos los char** que cotienen variables y vectores
        char * variables_array[] = {TAG_TIMESTAMP, TAG_TEMP, TAG_RAIN, TAG_HUM, NULL};
        char * values_array[] = {timestamp, temperature_char, rain_char, humidity_char, NULL};

        printf("%s: %s\n", TAG_TEMP, temperature_char);
        printf("%s: %s\n", TAG_RAIN, rain_char);
        printf("%s: %s\n", TAG_HUM, humidity_char);
        printf("%s\n", timestamp);

        // Guardamos en el log los valores
        writeVar(variables_array, values_array);

        usleep(refresh_rate * 1000000);
    }

    return 0;
}</pre>

&nbsp;

### Prueba

<a href="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/hum_test.png" data-rel="lightbox-1" title=""><img class=" size-full wp-image-600 aligncenter" src="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/hum_test.png?resize=518%2C277" alt="hum_test" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/hum_test.png?w=518&ssl=1 518w, https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/hum_test.png?resize=300%2C160&ssl=1 300w" sizes="(max-width: 518px) 100vw, 518px" data-recalc-dims="1" /></a>

&nbsp;

Observamos que la humedad se obtiene correctamente.

<a href="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_log.png" data-rel="lightbox-2" title=""><img class=" size-full wp-image-601 aligncenter" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_log.png?resize=264%2C333" alt="humidity_log" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_log.png?w=264&ssl=1 264w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/humidity_log.png?resize=238%2C300&ssl=1 238w" sizes="(max-width: 264px) 100vw, 264px" data-recalc-dims="1" /></a>

&nbsp;

También comprobamos que la escritura en el fichero LOG es correcta.

&nbsp;

## Referencias

[[1]](https://www.fasttech.com/product/1380900-fc-28-soil-humidity-detection-sensor-module) Sensor comprado.
  
<a href="https://img.fasttechcdn.com/138/1380900/1380900-2.jpg" data-rel="lightbox-3" title="">[2]</a> Imagen del sensor.