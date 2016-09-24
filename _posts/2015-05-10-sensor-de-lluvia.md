---
id: 573
title: 'PFG - Sensor de lluvia FR-04 y reorganización del código'
date: 2015-05-10T16:22:08+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=573
permalink: /proyectos/sensor-de-lluvia
categories:
  - Proyectos
tags:
  - agua
  - detección
  - i2c
  - lluvia
  - pfg
  - sensor
---
## Introducción

<del></del>El sensor de lluvia utilizado es un **FR-04** conectado a un divisor de tensión con un potenciómetro **LM393** . Las características según el fabricante son:

> 1. Voltage: 3.3-5V<img class="  wp-image-577 alignright" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=327%2C327" alt="rain" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?w=1500&ssl=1 1500w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=150%2C150&ssl=1 150w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=300%2C300&ssl=1 300w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=1024%2C1024&ssl=1 1024w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=144%2C144&ssl=1 144w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=160%2C160&ssl=1 160w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=320%2C320&ssl=1 320w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain.jpg?resize=900%2C900&ssl=1 900w" sizes="(max-width: 327px) 100vw, 327px" data-recalc-dims="1" />
  
> 2. Power indicator light, the output signal LED indicating lamp
  
> 3. TTL level output, TTL output signal for low level drive capacity of around 100MA, can directly drive the relay, a buzzer, a small fan, etc..
  
> 4. Sensitivity adjustment via potentiometer
  
> 5. No rain when the LED light output is high, the output level, go up, LED bright
  
> 6. The board and the control board is separate, convenient wire
  
> 7. A large area of the board, more conducive to detect the rain
  
> 8. The board is equipped with a positioning hole to facilitate installation
  
> 9. Control panel board size: 3*1.6 MM
  
> 10. A large area of raindrop detection board 5.4*4.0 MM
  
> 11. Connect cable: 200MM

Para el proyecto se utiliza la salida digital TTL, la cual conmuta cuando la tensión de salida es mayor o menor al valor del potenciómetro. Esta salida nos basta para saber si el sensor detecta agua (salida 0) o si está seco (salida 1). Por motivos de legibilidad se va a negar esta salida, de forma que sea 0 cuando está seco y 1 cuando detecta agua.

&nbsp;

## Conexión

<a href="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/rain_bb2.png" data-rel="lightbox-0" title=""><img class="  wp-image-576 aligncenter" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/rain_bb2.png?resize=698%2C459" alt="rain_bb" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/rain_bb2.png?w=1851&ssl=1 1851w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/rain_bb2.png?resize=300%2C197&ssl=1 300w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/rain_bb2.png?resize=1024%2C674&ssl=1 1024w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/rain_bb2.png?resize=900%2C592&ssl=1 900w" sizes="(max-width: 698px) 100vw, 698px" data-recalc-dims="1" /></a>

&nbsp;

Nota: el sensor no se corresponde con el del diagrama, pero sirve para indicar las conexiones.

El sensor tiene 4 salidas, de izquierda a derecha:

  * Pin 1: salida analógica que indica la cantidad de agua detectada. No se utiliza.
  * Pin 2: salida digital TTL la cual conmuta al detectar agua. Se conecta al puerto digital del arduino 50.
  * Pin 3: GND.
  * Pin 4: alimentación a 3.3V o 5V. Se escoge 3.3V para alimentar todos los sensores juntos.

&nbsp;

## Prueba del sensor

Para comprobar el sensor se escribe un programa sencillo para el Arduino. El programa establece comunicación serial, establece el pin digital 50 como entrada y realiza lecturas del pin cada 500 ms.

<pre class="lang:arduino decode:true">#define RAIN_SENSOR 50

void setup() {
  Serial.begin(9600);
  pinMode(RAIN_SENSOR, INPUT);
}

void loop() {
  int rain;
  rain = digitalRead(RAIN_SENSOR);
  Serial.println(rain);
  delay(500);
}</pre>

Funciona correctamente, comprobándose que la salida conmuta al echar unas gotas de agua sobre el sensor.

&nbsp;

## Comunicación I2C

Para comprobar el funcionamiento de la placa en relación a la obtención de los valores de los sensores y su posterior comunicación a la Raspberry Pi mediante I2C se modifica el programa que se usó para probar el sensor de temperatura.

Cuando el Arduino reciba una cadena de caracteres que sea &#8220;RAIN&#8221;, enviará el valor como respuesta.

&nbsp;

### Arduino

<pre class="lang:arduino decode:true ">#include <Wire.h>
#include <math.h>
 
#define SLAVE_ADDRESS 0x08
#define TAG_TEMP "TEMP"
#define TAG_RAIN "RAIN"
#define TEMPERATURE_SENSOR 0
#define RAIN_SENSOR 50

String var;
char c;

double temperature;
int int_temperature;
int rain;

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
}
</pre>

&nbsp;

### Programa para la Raspberry Pi

Se desarrolla un programa que pretende ser el hilo principal de ejecución de la Raspberry Pi. Este programa:

  * Lee en el fichero _vars/REFRESH_RATE_ el valor de actualización de los valores de los sensores con la función **char \* readVar(char \* variable)**.</p> 
  * Lee los valores de los sensores de temperatura y lluvia mediante la función **int readSensor(int device, char * sensor, uint nBytesSend, uint nBytesReceive)**, la cual recibe como parámetros la dirección del dispositivo a la que debe conectarse, el nombre del sensor, los bytes que se envían como petición y los bytes que se espera recibir.

  * Obtiene la fecha y hora (timestamp) para adjuntarla a los valores obtenidos anteriormente con la función **char * getTime()**.

  * Convierte los valores de los sensores a char * para poder procesarlos y almacenarlos en un fichero.

  * Genera un array con el nombre de las variables y otro con sus valores y luego los escribe en el fichero _vars/VARS_ mediante la función **int writeVar(char \* variables\_array[], char \* values\_array[])**. Este programa además genera un encabezado al fichero LOG si está vacío.

Para que el valor de timestamp sea correcto, es necesario ajustar la zona horaria de la Raspberry Pi, para ello ejecutamos:

<pre class="lang:sh decode:true ">> timedatectl set-timezone Europe/Madrid</pre>

&nbsp;

#### Programa main.c

<pre class="lang:c decode:true">#include <stdio.h>
#include <stdlib.h>
#include "functions.h"

#define ARDUINO 0x04
#define TI 0x08

#define TAG_REFRESH_RATE "REFRESH_RATE"
#define TAG_TIMESTAMP "TIMESTAMP"
#define TAG_TEMP "TEMP"
#define TAG_RAIN "RAIN"

int main() {
    char * buf;

    int refresh_rate;

    char * timestamp;

    uint nBytesTAG_TEMP = sizeof(TAG_TEMP)-1; // no enviamos el caracter '\0'
    uint nBytesTAG_RAIN = sizeof(TAG_RAIN)-1;
    uint nBytesTEMP = 4;
    uint nBytesRAIN = 1;

    float temperature;
    uint rain;

    char temperature_char[] = {0};
    char rain_char[] = {0};

    buf = readVar(TAG_REFRESH_RATE);
    refresh_rate = atoi((char *) &buf);
    printf("%s: %d\n", TAG_REFRESH_RATE, refresh_rate);

    for(;;) {
        
        // Leemos los valores se los sensores
        temperature = readSensor(TI, TAG_TEMP, nBytesTAG_TEMP, nBytesTEMP) / 100.00;
        usleep(50000);
        rain = readSensor(TI, TAG_RAIN, nBytesTAG_RAIN, nBytesRAIN);

        timestamp = getTime();

        // Convertimos los valores a char*
        sprintf(temperature_char, "%.2f", temperature);
        sprintf(rain_char, "%d", rain);

        // Creamos los char** que cotienen variables y vectores
        char * variables_array[] = {TAG_TIMESTAMP, TAG_TEMP, TAG_RAIN, NULL};
        char * values_array[] = {timestamp, temperature_char, rain_char, NULL};

        printf("%s: %s\n", TAG_TEMP, temperature_char);
        printf("%s: %s\n", TAG_RAIN, rain_char);
        printf("%s\n", timestamp);

        // Guardamos en el log los valores
        writeVar(variables_array, values_array);

        usleep(refresh_rate * 1000000);
    }

    return 0;
}</pre>

Se compila con el comando:

<pre class="lang:sh decode:true">> gcc -o main main.c functions.c</pre>

&nbsp;

#### Programa functions.h

Contiene las cabeceras de las funciones auxiliares descritas anteriormente.

<pre class="lang:c decode:true ">extern char * ruta(char * variable);
extern char * readVar(char * variable);
extern int writeVar(char * variables_array[], char * values_array[]);
extern char * getTime();
extern int readSensor(int device, char * sensor, uint nBytesSend, uint nBytesReceive);</pre>

&nbsp;

#### Programa functions.c

Contiene las funciones auxiliares.

<pre class="lang:c decode:true ">#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <fcntl.h> // open modo O_RDWR
#include <linux/i2c-dev.h>
#include "functions.h"

char * ruta(char * variable) {
    char * temp;
    char * directorio = "vars/";
    if((temp = malloc(strlen(directorio)+strlen(variable)+1)) != NULL) {
        temp[0]='\0';
        strcat(temp, directorio);
        strcat(temp, variable);
        return temp;
    }
}

char * readVar(char * variable) {
    FILE * f;
    char * nombre_fichero;
    char * value = malloc(10 * sizeof(char));

    nombre_fichero = ruta((char *) variable);
    f = fopen((char *) nombre_fichero,"r");

    if (f != NULL) {
        fscanf(f, "%s", &value);
        fclose(f);
    }
    return value;
}

int writeVar(char * variables_array[], char * values_array[]) {
    FILE * f;
    char * nombre_fichero;
    int i;

    nombre_fichero = ruta("VARS");

    f = fopen(nombre_fichero, "a+");

    if (f != NULL) {

        for (i=0; variables_array[i] != NULL; i++);
        int n_elements = i;

        // Vemos si hay que imprimir el header
        fseek(f, 0, SEEK_END);
        if (ftell(f) == 0) {
            for (i=0; i<n_elements; i++) {
                if(i != n_elements-1)
                    fprintf(f, "%s,", variables_array[i]);
                else
                    fprintf(f, "%s\n", variables_array[i]);
            }
        }

        // Escribimos las variables
        for (i=0; i < n_elements; i++) {
            if (i != n_elements-1)
                fprintf(f, "%s,", values_array[i]);
            else
                fprintf(f, "%s\n", values_array[i]);
        }

        fclose(f);
        return 0;
    }
    else
        return -1;
}

char * getTime() {
    time_t current_time;
    char * c_time_string;

    /* Obtain current time as seconds elapsed since the Epoch. */
    current_time = time(NULL);

    /* Convert to local time format. */
    c_time_string = (char *) ctime(&current_time);
    c_time_string[strlen(c_time_string)-1] = '\0';

    return c_time_string;
}

int readSensor(int device, char * sensor, uint nBytesSend, uint nBytesReceive) {
    int fd;
    char buf[4];
    float value = -100;
    
    if ((fd = open("/dev/i2c-0", O_RDWR)) >= 0) {
    
        if (ioctl(fd, I2C_SLAVE, device) >= 0) {
    
            if (write(fd, sensor, nBytesSend) == nBytesSend) {

                usleep(50000);

                if (read(fd, &buf, nBytesReceive) == nBytesReceive) {
                    value = atoi(buf);
                }
            }
        }
    }

    close(fd);
    return value;
}</pre>

&nbsp;

### Prueba del programa

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain_test.png" data-rel="lightbox-1" title=""><img class=" size-full wp-image-583 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain_test.png?resize=641%2C226" alt="rain_test" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain_test.png?w=641&ssl=1 641w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/rain_test.png?resize=300%2C106&ssl=1 300w" sizes="(max-width: 641px) 100vw, 641px" data-recalc-dims="1" /></a>

Comprobamos que funciona correctamente.

&nbsp;

## Referencias

[[1]](http://www.amazon.com/Detector-Module-Sensor-Arduino-Interface/dp/B00EEWCSRI/ref=pd_sim_sbs_328_1?ie=UTF8&refRID=07SRGGDJW112Q4JRY8GW) Módulo de detección de lluvia comprado.
  
<a href="http://ecx.images-amazon.com/images/I/71suNGOx2IL._SL1500_.jpg" data-rel="lightbox-2" title="">[2]</a> Imagen del sensor.
  
[[3]](https://wiki.archlinux.org/index.php/Time) Ajustar la zona horaria en la Raspberry Pi.

&nbsp;
