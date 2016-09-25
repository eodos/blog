---
id: 442
title: 'PFG - Conexión I2C entre la Raspberry Pi y el Arduino'
date: 2014-11-11T22:36:46+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=442
permalink: /proyectos/conexion-i2c
categories:
  - Proyectos
tags:
  - arduino
  - comunicación
  - i2c
  - pfg
  - raspberry
---
<img class="size-medium wp-image-443 aligncenter" src="https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c-300x100.png?resize=300%2C100" alt="I2C" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2014/11/i2c.png?resize=300%2C100&ssl=1 300w, https://i1.wp.com/eodos.net/wp-content/uploads/2014/11/i2c.png?w=428&ssl=1 428w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />

<p style="text-align: justify;">
  Se ha escogido el protocolo I2C de conexión debido a la facilidad que otorga a la hora de escalar el proyecto, ya que admite hasta 127 periféricos conectados entre sí. Las Raspberry Pi funciona a 3.3V y el Arduino Mega a 5V, lo cual no es un problema si usamos la Raspberry de maestro, ya que sus 3.3V de salida alta entran dentro del umbral de entradas altas del Arduino.
</p>

<h1 style="text-align: justify;">
  Conexión
</h1>

<p style="text-align: justify;">
  Los puertos I2C de la Raspberry Pi ya incluyen resistencias Pull-Up de 1.8K. En el Arduino Mega son los puertos 20 (SDA) y 21 (SCL). Además, las placas deben compartir alimentación y tierra. Al tener alimentaciones distintas se mantienen separadas. Las conexiones quedan de esta forma:
</p>

<a href="https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png" data-rel="lightbox-0" title=""><img class="size-medium wp-image-444 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1-300x297.png?resize=300%2C297" alt="Conexión I2C" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png?resize=300%2C297&ssl=1 300w, https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png?resize=150%2C150&ssl=1 150w, https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png?resize=1024%2C1014&ssl=1 1024w, https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png?resize=144%2C144&ssl=1 144w, https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png?resize=900%2C891&ssl=1 900w, https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/i2c1.png?w=1266&ssl=1 1266w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

<p style="text-align: justify;">
  A continuación vamos a configurar la placa Raspberry Pi para usar sus dos puertos I2C. En primer lugar instalamos el paquete <em>i2c-tools:</em>
</p>

<pre class="lang:sh decode:true ">> pacman -S i2c-tools</pre>

<p style="text-align: justify;">
   Después añadimos el módulo para que se cargue con el kernel, añadiendo "i2c-dev" al fichero <em>/etc/modules-load.d/raspberrypi.conf</em>
</p>

<p style="text-align: justify;">
  Reiniciamos y comprobamos que funciona.
</p>

<pre class="lang:sh decode:true ">> ls /dev/i2c*
/dev/i2c-0</pre>

Leemos del puerto I2C:

<pre class="lang:sh decode:true">> i2cdetect -y 0
0 1 2 3 4 5 6 7 8 9 a b c d e f
00: -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
</pre>

<p style="text-align: justify;">
  Todo funciona correctamente. Ahora vamos a probar a enviar una señal de la Raspberry al Arduino y que este la devuelva.
</p>

&nbsp;

# Programa para el Arduino

<p style="text-align: justify;">
  Vamos a crear un programa que reciba un número por el puerto I2C y lo reenvie. Además, si el número es el 1 encenderá o apagará el LED que tiene integrado en la placa.
</p>

<p style="text-align: justify;">
  Hacemos uso de la libería <strong>Wire.h </strong>y de las llamadas Wire.begin(puerto), Wire.onReceive(funcion), Wire.onRequest(funcion), Wire.available(), Wire.read(), y Wire.write().
</p>

<p style="text-align: justify;">
  El programa queda así:
</p>

<pre class="lang:c decode:true ">#include <Wire.h>
 
#define SLAVE_ADDRESS 0x04

int number = 0;
int state = 0;
 
// configuracion inicial
void setup() {
    pinMode(13, OUTPUT);
    // inicializamos i2c como esclavo
    Wire.begin(SLAVE_ADDRESS);
    // llamadas para la comunicacion
    Wire.onReceive(receiveData);
    Wire.onRequest(sendData);
}
 
 // bucle principal (vacio)
void loop() {}

// llamada para recibir datos
void receiveData(int byteCount) {
    while(Wire.available()) {
        number = Wire.read();
        if (number == 1) {
            if (state == 0) {
                digitalWrite(13, HIGH); // set the LED on
                state = 1;
            } 
            else{
                digitalWrite(13, LOW); // set the LED off
                state = 0;
            }
        }
    }
}
 
// llamada para enviar datos
void sendData() {
    Wire.write(number);
}</pre>

&nbsp;

# Programa para la Raspberry Pi

<p style="text-align: justify;">
  Este programa escrito en C se conectará con la placa Arduino Mega mediante el protocolo I2C, enviará un 1 e imprimirá por pantalla la respuesta que reciba del Arduino.
</p>

<pre class="lang:c decode:true">#include <stdio.h>
#include <stdlib.h> // exit
#include <fcntl.h> // open modo O_RDWR
#include <linux/i2c-dev.h>

#define ADDRESS 0x04

int main() {
    int file;
    unsigned char cmd[16];
    int enviado = 1;
    char buff[1];
    int recibido;
    
    printf("I2C: Conectando...\n");
    if ((file = open("/dev/i2c-0", O_RDWR)) < 0) {
        printf("I2C: error de acceso");
        exit(1);
    }
    
    printf("I2C: Accediendo al dispositivo \n");
    if (ioctl(file, I2C_SLAVE, ADDRESS) < 0) {
        printf("I2C: error al acceder al esclavo en 0x%x", ADDRESS);
        exit(1);
    }
    
    printf("Enviado %d\n", enviado);
    cmd[0] = enviado;
    if (write(file, cmd, 1) == 1) {
        usleep(10000);
        if (read(file, buff, 1) == 1) {
            recibido = (int) buff[0];
            printf("Recibido %d\n", recibido);
        }
    }
    
    printf("I2C: Terminando...\n");
    close(file);
    return (EXIT_SUCCESS);
}</pre>

Compilamos el programa y lo ejecutamos.

<pre class="lang:sh decode:true ">> ./i2c
I2C: Conectando...
I2C: Accediendo al dispositivo
Enviado 1
Recibido 1
I2C: Terminando...</pre>

<p style="text-align: justify;">
  Cada vez que ejecutamos el programa desde la Raspberry Pi, el LED integrado en el Arduino Mega se enciende o apaga correctamente.
</p>

<p style="text-align: justify;">
  Detectando de nuevo las conexiones I2C vemos que el Arduino está correctamente conectado en el puerto 4.
</p>

<pre class="lang:sh decode:true ">> i2cdetect -y 0
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- 04 -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
</pre>