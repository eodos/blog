---
id: 490
title: 'PFG - Comunicación I2C de la placa TI Stellaris Launchpad'
date: 2015-02-28T17:57:30+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=490
permalink: /proyectos/i2c-ti-launchpad
categories:
  - Proyectos
tags:
  - energia
  - i2c
  - launchpad
  - pfg
  - stellaris
  - texas instrument
  - ti
---
## Instalación de drivers y prueba de la placa

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/02/stellaris.jpg" data-rel="lightbox-0" title=""><img class=" size-medium wp-image-493 alignleft" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/02/stellaris-300x214.jpg?resize=300%2C214" alt="stellaris" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/02/stellaris.jpg?resize=300%2C214&ssl=1 300w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/02/stellaris.jpg?w=750&ssl=1 750w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

Para instalar los drivers de la placa en el ordenador que se va a usar para el desarrollo de su programa, se ha seguido [esta guía](http://www.ti.com/lit/ml/spmu287b/spmu287b.pdf).

Lo primero es descargar los drivers de [aquí](http://www.ti.com/tool/stellaris_icdi_drivers).

Como no están firmados digitalmente, es necesario habilitar las directivas de grupo local (en Windows 8) para permitir la instalación de este tipo de drivers.

Para usar el entorno de desarrollo [Energia](http://energia.nu/) primero hay que descargar los [drivers](http://energia.nu/wordpress/wp-content/uploads/2014/12/xds100.zip). Una vez descargado el entorno de desarrollo escribimos un programa de prueba que haga parpadear el LED que lleva integrado la placa.

<pre class="lang:c decode:true">#define LED RED_LED
void setup() {                
  pinMode(LED, OUTPUT);     
}

void loop() {
  digitalWrite(LED, HIGH);   
  delay(1000);               
  digitalWrite(LED, LOW);    
  delay(1000);
}</pre>

Para conectar correctamente el IDE con la placa conectada por USB, seleccionamos el puerto de comunicación **COM4** y como placa de desarrollo la **Launchpad (Stellaris) w/ lm4f120 (80MHz)**. Una vez cargado el programa funciona correctamente.

## Comunicación I2C

Pese a que esta placa y la Arduino Mega son de fabricantes distintos, podemos usar la misma librería en ambos, la **Wire.h** para realizar la comunicación I2C de forma sencilla.

Lo primero es realizar la conexión física entre la placa Texas Instrument y la Raspberry Pi. En el caso de esta SBM, los pines son:

Pin PD_0 => SCL
  
Pin PD_1 => SDA

El programa que cargamos en la placa de Texas Instrument es similar al realizado anteriormente, pero cambiando la **dirección I2C** que quiere utilizar el dispositivo.

<pre class="lang:c decode:true ">#include <Wire.h>

#define LED RED_LED
#define SLAVE_ADDRESS 0x08

int number = 0;
int state = 0;

void setup() {                
  pinMode(LED, OUTPUT);     
  Wire.begin(SLAVE_ADDRESS);
  Wire.onReceive(receiveData);
  Wire.onRequest(sendData);
}

void loop() {}

void receiveData(int byteCount) {
  while(Wire.available()) {
    number = Wire.read();
    if (number == 1) {
      if (state == 0) {
        digitalWrite(LED, HIGH);
        state = 1;
      }
      else {
        digitalWrite(LED, LOW);
        state = 0;
      }
    }
  }
}

void sendData() {
  Wire.write(number);
}</pre>

Para probar que funciona, reutilizamos el script **i2c.cgi** de la Raspberry Pi modificando la dirección del Arduino (0x04) por la de la Texas Instrument (0x08). Comprobamos que la Raspberry Pi detecta la placa TI.

<pre class="lang:sh decode:true ">> i2cdetect -y 0
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- 08 -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --</pre>

## Documentación consultada

  * [[1]](http://dronespersonalizados.blogspot.com.br/2014/06/stellaris-tiva-i2c-leds.html) Para ver el esquemático de la comunicación física entre la TI y la Raspberry Pi

  * [[2]](http://www.energia.nu/) El entorno de desarrollo usado para la placa

  * [[3]](http://www.ti.com/ww/en/launchpad/launchpad.html) Documentación acerca de como instalar los drivers

  * [[4]](http://www.ti.com/tool/Ek-LM4F120XL) Imagen