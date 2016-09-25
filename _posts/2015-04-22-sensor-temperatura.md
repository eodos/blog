---
id: 552
title: 'PFG - Sensor de Temperatura KY-013'
date: 2015-04-22T16:33:16+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=552
permalink: /proyectos/sensor-temperatura
categories:
  - Proyectos
tags:
  - arduino
  - i2c
  - pfg
  - sensor
  - temperatura
---
## Introducción

Un termistor es un tipo de resistor cuya resistencia depende de los cambios de temperatura. Un termistor se puede clasificar en dos grupos:

  * **PTC** (positive temperature coefficient). La resistencia aumenta con el incremento de la temperatura.
  * **NTC** (negative temperature coefficient). La resistencia disminuye al aumentar la temperatura.

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Thermistor_RT.png" data-rel="lightbox-0" title=""><img class=" size-full wp-image-553 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Thermistor_RT.png?resize=734%2C391" alt="Thermistor_RT" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Thermistor_RT.png?w=734&ssl=1 734w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Thermistor_RT.png?resize=300%2C160&ssl=1 300w" sizes="(max-width: 734px) 100vw, 734px" data-recalc-dims="1" /></a>

&nbsp;

La relación entre la resistencia y la temperatura se puede aproximar así:

<p style="text-align: center;">
  <img src='https://s0.wp.com/latex.php?latex=R+%3D+R_%7BRef%7D%5Ccdot+e%5E%7B%5Cbeta+%5Cleft+%28+%5Cfrac%7B1%7D%7BT%7D+-+%5Cfrac%7B1%7D%7BT_%7BRef%7D%7D+%5Cright+%29+%7D&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='R = R_{Ref}\cdot e^{\beta \left ( \frac{1}{T} - \frac{1}{T_{Ref}} \right ) }' title='R = R_{Ref}\cdot e^{\beta \left ( \frac{1}{T} - \frac{1}{T_{Ref}} \right ) }' class='latex' />
</p>

Donde <img src='https://s0.wp.com/latex.php?latex=R&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='R' title='R' class='latex' /> es la resistencia del termistor a una temperatura  <img src='https://s0.wp.com/latex.php?latex=T&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='T' title='T' class='latex' />(en Kelvin),  <img src='https://s0.wp.com/latex.php?latex=R_%7BRef%7D&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='R_{Ref}' title='R_{Ref}' class='latex' />es la resistencia del material a una temperatura <img src='https://s0.wp.com/latex.php?latex=T_%7BRef%7D&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='T_{Ref}' title='T_{Ref}' class='latex' />, y  <img src='https://s0.wp.com/latex.php?latex=%5Cbeta&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='\beta' title='\beta' class='latex' />es una constante específica del material.

Despejando la temperatura:

<p style="text-align: center;">
  <img src='https://s0.wp.com/latex.php?latex=%5Cfrac%7B1%7D%7BT%7D+%3D+A+%2B+B%5Ccdot+ln%28R%29+%2B+C+%5Bln+%28R%29+%5D%5E3&#038;bg=ffffff&#038;fg=000000&#038;s=0' alt='\frac{1}{T} = A + B\cdot ln(R) + C [ln (R) ]^3' title='\frac{1}{T} = A + B\cdot ln(R) + C [ln (R) ]^3' class='latex' />
</p>

Los parámetros A, B y C son proporcionados por el fabricante. En el caso del termistor **Keynes KY-013**, dichos valores son:

  * A = 0.001129148
  * B = 0.000234125
  * C = 0.0000000876741

&nbsp;

## Conexión del sensor

El termistor tiene tres puertos, de izquierda a derecha:

  * **(S)**. La salida analógica. Lo conectamos a cualquier puerto analógico del Arduino, por ejemplo **A0**.
  * **(+)**. Entrada de 5V.
  * **(-)**. Conexión a tierra.

Al conectarlo y probarlo con el programa del siguiente apartado nos damos cuenta de que la salida decrece al aumentar la temperatura y viceversa, por lo que hay que intercambiar los pines 2 y 3 (5V y GND), para que funcione correctamente.

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/ky013_bb.png" data-rel="lightbox-1" title=""><img class="  wp-image-565 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/ky013_bb.png?resize=612%2C403" alt="ky013_bb" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/ky013_bb.png?w=1851&ssl=1 1851w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/ky013_bb.png?resize=300%2C197&ssl=1 300w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/ky013_bb.png?resize=1024%2C674&ssl=1 1024w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/ky013_bb.png?resize=900%2C592&ssl=1 900w" sizes="(max-width: 612px) 100vw, 612px" data-recalc-dims="1" /></a>

&nbsp;

## Programa para el Arduino

El fabricante nos proporciona un pequeño programa para probar el sensor, aplicando la fórmula anterior:

<pre class="lang:arduino decode:true ">#include <math.h>

void setup() {
  Serial.begin(9600);
}

void loop() {
  double temperature;
  temperature = Thermister(analogRead(0));
  Serial.print(temperature);
  Serial.println(" C");
  delay(500);
}

// A partir de la salida del termistor calculamos la temperatura
double Thermister(int voltaje) {
  double Temp;
  Temp = log(((10240000/voltaje) - 10000));
  Temp = 1 / (0.001129148 + (0.000234125 + (0.0000000876741 * Temp * Temp ))* Temp );
  Temp = Temp - 273.15;
  return Temp;
}</pre>

Como comprobamos leyendo del puerto serial, el sensor funciona correctamente.

<a href="https://i0.wp.com/eodos.net/wp-content/uploads/2015/04/temperatura_arduino.png" data-rel="lightbox-2" title=""><img class=" size-full wp-image-564 aligncenter" src="https://i0.wp.com/eodos.net/wp-content/uploads/2015/04/temperatura_arduino.png?resize=418%2C380" alt="temperatura_arduino" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2015/04/temperatura_arduino.png?w=418&ssl=1 418w, https://i0.wp.com/eodos.net/wp-content/uploads/2015/04/temperatura_arduino.png?resize=300%2C273&ssl=1 300w" sizes="(max-width: 418px) 100vw, 418px" data-recalc-dims="1" /></a>

&nbsp;

## Petición y envío por I2C de la temperatura

El siguiente programa tiene como objetivo que la Raspberry Pi realice una petición por I2C de la temperatura enviando la cadena "TEMP" y el Arduino, al recibirla le responda con el valor de la temperatura.

&nbsp;

### Arduino

En primer lugar, al recibir información por I2C y al tratarse de una cadena de caracteres, utilizamos la librería **<Wire.h>** y la clase **String** de Arduino para concatenar los caracteres que se van recibiendo en Bytes. A continuación se compara la cadena con lo que se espera recibir y en caso de coincidencia se lee la temperatura del sensor con la función anterior. A continuación hay que convertir este valor **double** en un **char array** para poder contabilizar los bytes que se envían por I2C. Para ello primero se convierte en un **long int**, previamente habiendo multiplicado la temperatura por 100 para mantener 2 dígitos decimales de precisión, se convierte a continuación en un **String** y se utiliza el método **toCharArray** para realizar la conversión final a **char array**, el cual es enviado por I2C.

<pre class="lang:arduino decode:true">#include <Wire.h>
#include <math.h>
 
#define SLAVE_ADDRESS 0x04

String TAG_TEMP = "TEMP";
String var;
char c;
int value;
double temperature;
String temp;
char char_array[] = "";
 
// configuracion inicial
void setup() {
  // puerto serie
  Serial.begin(9600);
  
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
    
  if (var == TAG_TEMP) {
    Serial.println("TEMP request received");
    temperature = Thermister(analogRead(0)) *100;
    Serial.println(temperature);
       
    // Convert double to char*
    value = (long int) temperature; 
    temp = (String) value;
    unsigned int temp_length = temp.length()+1;
    ((String)temp).toCharArray(char_array, temp_length);
  }
}
 
// llamada para enviar datos
void sendData() {
  Wire.write(char_array);
  Serial.print("sent: ");
  Serial.println(char_array);
}

double Thermister(int voltaje) {
  double Temp;
  Temp = log(((10240000/voltaje) - 10000));
  Temp = 1 / (0.001129148 + (0.000234125 + (0.0000000876741 * Temp * Temp ))* Temp );
  Temp = Temp - 273.15;
  return Temp;
}</pre>

&nbsp;

### Raspberry Pi

Existe una librería llamada **WiringPi** que permite el envío y recepción de datos en la Raspberry Pi con comandos similares a los de la librería **<Wire.h>** del Arduino. Sin embargo el comando **int wiringPiI2CWrite (int fd, int data)** solo admite el paso de un entero como parámetro para su envío por I2C, y dado que queremos enviar la cadena de caracteres **TEMP** no nos vale. Por tanto lo realizamos con las llamadas **read** y **write**.

El propósito de programa para poder probar la recepción de la temperatura por I2C realizará una única petición al ejecutarse en forma de script CGI. En las siguientes versiones realizará comprobaciones periódicas con una frecuencia que leerá de un fichero.

El programa establece en primer lugar la conexión a través del puerto **/dev/i2c-0** con los comandos **open** y **ioctl**. A continuación escribe en el puerto la cadena de caracteres "TEMP" y si todos los bytes se han enviado correctamente (4), espera un tiempo y realiza la operación de lectura en un buffer array char. Cuando recibe los 4 bytes de la temperatura, realiza una transformación a entero con el comando **atoi** para a continuación convertirlo en flotante y así recuperar los dos dígitos decimales.

Además resulta conveniente anotar la hora a la que se ha realizado la lectura, por lo tanto se calcula con una función que se ha creado llamada **char * getTime()**, la cual hace uso de la librería **<time.h>**.

<pre class="lang:c decode:true">#include <stdio.h>
#include <stdlib.h> // exit
#include <fcntl.h> // open modo O_RDWR
#include <linux/i2c-dev.h>
#include <time.h>

#define ARDUINO_ADDRESS 0x04
#define TI_ADDRESS 0x08


char* getTime();

int main() {
    int fd;
    char TAG_TEMP[] = "TEMP";
    uint nBytesTAG_TEMP = 4;
    uint nBytesTEMP = 4;
    float temperature;
    char buf[4];
    int value;
    
    printf("Content-Type: text/html\n\n");
    printf("<html>");
    printf("<body>");
    
    printf("I2C: Conectando...<br/>");
    if ((fd = open("/dev/i2c-0", O_RDWR)) < 0) {
        printf("I2C: error de acceso");
        exit(1);
    }
    
    printf("I2C: Accediendo al Arduino...<br/><br/>");
    if (ioctl(fd, I2C_SLAVE, ARDUINO_ADDRESS) < 0) {
        printf("I2C: error al acceder al esclavo en 0x%x", ARDUINO_ADDRESS);
        exit(1);
    }
    
    // TEMPERATURE request
    printf("var enviada: %s<br/>", TAG_TEMP);
    if (write(fd, TAG_TEMP, nBytesTAG_TEMP) == nBytesTAG_TEMP) {

        usleep(10000);

        if (read(fd, &buf, nBytesTEMP) == nBytesTEMP) {

            value = atoi(buf);
            printf("valor: %d<br/>", value);
            temperature = (float) value / (float) 100.00;
            printf("temperatura: %.2f C<br/>", temperature);
            printf("%s<br/>", getTime());
        }
    }
    else printf("envio incorrecto<br/>");

    printf("<br/>I2C: Terminando...");
    printf("</body></html>");
    close(fd);
    return (EXIT_SUCCESS);
}

char* getTime() {
    time_t current_time;
    char* c_time_string;

    /* Obtain current time as seconds elapsed since the Epoch. */
    current_time = time(NULL);

    /* Convert to local time format. */
    c_time_string = ctime(&current_time);

    return c_time_string;
}</pre>

Probamos el Script:

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura1.png" data-rel="lightbox-3" title=""><img class=" size-full wp-image-569 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura1.png?resize=821%2C376" alt="Captura" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura1.png?w=821&ssl=1 821w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura1.png?resize=300%2C137&ssl=1 300w" sizes="(max-width: 821px) 100vw, 821px" data-recalc-dims="1" /></a>

&nbsp;

## Referencias

  * [[1]](http://es.aliexpress.com/item/Analog-Temperature-Sensor-Module-For-Arduino-AVR-PIC/1773738744.html?recommendVersion=1) Sensor de temperatura comprado.
  * [[2]](http://eagerlearning.org/practice/sensors/temperature-sensor/) Información sobre el sensor de temperatura.
  * <a href="http://eagerlearning.org/wp-content/uploads/2015/01/Thermistor_RT.png" data-rel="lightbox-4" title="">[3]</a> Gráfica de la relación entre T y R en un termistor.
  * [[4]](http://arduino.cc/en/Reference/WireRead) Comando Wire.read de Arduino.
  * [[5]](http://arduino.cc/en/Tutorial/MasterWriter) Ejemplo de transmisión de datos en Arduino.
  * [[6]](http://www.arduino.cc/en/Reference/StringObject) Strings en Arduino.
  * [[7]](http://www.cplusplus.com/reference/ctime/) Librería <time.h> de C.
  * [[8]](https://projects.drogon.net/raspberry-pi/wiringpi/i2c-library/) Librería WiringPi.
  * \[[9]\](http://stackoverflow.com/questions/7383606/converting-an-int-or-string-to-a-char-array-
  
    on-arduino) Conversión de entero a char* en Arduino.