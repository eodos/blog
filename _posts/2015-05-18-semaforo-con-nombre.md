---
id: 615
title: 'PFG - Semáforo con nombre'
date: 2015-05-18T18:04:49+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=615
permalink: /proyectos/semaforo-con-nombre
categories:
  - Proyectos
tags:
  - c
  - pfg
  - semaforo
  - semaforo con nombre
  - sleep
  - timedwait
---
## Introducción

El programa **main.c** realiza una espera del tiempo indicado en el fichero ```vars/REFRESH_RATE``` usando la instrucción ```sleep()```. El uso de esta instrucción es adecuada al no suponer una espera activa del proceso, pero no nos permite interrumpir la espera si, por ejemplo, desde la aplicación Android solicitamos una actualización inmediata de los valores de los sensores.

Para ellos podemos usar la instrucción de POSIX ```timedwait``` (implementada en ArchLinux) la cual funciona como un semáforo combinado con una instrucción sleep. Espera el tiempo indicado antes de continuar o hasta que el semáforo que tiene como parámetro se abra. Como queremos que el semáforo se pueda abrir desde la aplicación Android (mediante un script CGI escrito en C) tenemos que usar un semáforo con nombre, que se ubique en la memoria compartida del sistema operativo ```/dev/shm.``` Esta instrucción se encuentra en la librería ```<semaphore.h>``` que se debe importar.


## Programas de prueba

Para probar el funcionamiento de la instrucción timedwait, se crea un programa de prueba que acceda al semáforo, asigne un tiempo de espera de 10 segundos y espere con la instrucción ```sem_wait()```. Por otro lado se crea un script CGI que acceda al semáforo y lo abra con ```sem_post()```.


### test.c

Se hace necesario, además, ejecutar la instrucción ```chmod``` sobre el semáforo con nombre ya que los permisos no se asignan correctamente al crear el semáforo.

```c
#include <semaphore.h>
#include <time.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include <fcntl.h> // O_CREAT

sem_t * sem;

int main() {

  char sem_name[] = "miSemaforo";
  char shm_path[] = "/dev/shm/sem.";
  char sem_path[20];
  strcpy(sem_path, shm_path);
  strcat(sem_path, sem_name);

  sem = sem_open(sem_name, O_CREAT, 0666, 0);

  chmod(sem_path, 0666);

  static struct timespec time_to_wait = {0, 0};

  for (;;) {

    time_to_wait.tv_sec = time(NULL) + 10;

    printf("Espero...\n");
    sem_timedwait(sem, &time_to_wait);
    printf("He salido\n");  
  }

  sem_close(sem);
  sem_unlink(sem_name);
}
```

Se compila con el comando:

```bash
$ gcc -o test test.c -lpthread
```


### open_semaphore.cgi

```c
/* Este script cgi abre un semáforo con nombre */

#include <semaphore.h>
#include <time.h>
#include <stdio.h>
#include <stdlib.h>

#include <fcntl.h> // O_CREAT

sem_t * sem;

int main() {

  char myName[] = "miSemaforo";

  int valor;

  printf("Content-Type: text/html\n\n");
  
  sem = sem_open(myName, O_CREAT, 0666, 0);

  sem_getvalue(sem, &valor);
  printf("Voy a abrir...%d\n", valor);
  sem_post(sem);
  sem_getvalue(sem, &valor);
  printf("Abierto. %d\n", valor);

  sem_close(sem);

}
```

Se compila con el comando:

```bash
$ gcc -o open_semaphore.cgi open_semaphore.c -lpthread
```

Comprobamos que funciona correctamente. El programa continua ejecutándose a los 10 segundos o al ejecutar el script.

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore.png" data-rel="lightbox-0" title=""><img class=" size-full wp-image-617 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore.png?resize=353%2C130" alt="semaphore" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore.png?w=353&ssl=1 353w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore.png?resize=300%2C110&ssl=1 300w" sizes="(max-width: 353px) 100vw, 353px" data-recalc-dims="1" /></a>

<a href="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore_2.png" data-rel="lightbox-1" title=""><img class=" size-full wp-image-618 aligncenter" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore_2.png?resize=503%2C111" alt="semaphore_2" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore_2.png?w=503&ssl=1 503w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/semaphore_2.png?resize=300%2C66&ssl=1 300w" sizes="(max-width: 503px) 100vw, 503px" data-recalc-dims="1" /></a>


## Adaptación al programa main.c

Ahora se añade el semáforo al programa principal de la Raspberry Pi. En cada ejecución del bucle principal leemos el valor de la tasa de refresco (por si el usuario lo ha modificado desde la aplicación Android) y actualizamos el semáforo con nombre para que espere el valor indicado.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h> // O_CREAT
#include <semaphore.h>
#include "functions.h"

#define ARDUINO 0x04
#define TI 0x08

#define TAG_REFRESH_RATE "REFRESH_RATE"
#define TAG_FORECAST "FORECAST"
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
  int forecast;

  char temperature_char[] = {0};
  char rain_char[] = {0};
  char humidity_char[] = {0};
  char forecast_char[] = {0};

  int pid, status;

  sem_t * sem;

  char sem_name[] = "miSemaforo";
  char shm_path[] = "/dev/shm/sem.";
  char sem_path[20];
  strcpy(sem_path, shm_path);
  strcat(sem_path, sem_name);

  sem = sem_open(sem_name, O_CREAT, 0666, 0);

  chmod(sem_path, 0666);

  static struct timespec time_to_wait = {0, 0};

  // Creamos el proceso hijo y le indicamos que ejecute el script para obtener la predicción meteorológica
  pid = fork();

  if (pid == 0) {
    execlp("python", "python", "weather_forecast.py", NULL);
    exit(0);
  }

  // Proceso padre
  else {

    for(;;) {

      // Leemos la tasa de refresco
      buf = readVar(TAG_REFRESH_RATE);
      refresh_rate = atoi((char *) &buf);
      printf("%s: %d\n", TAG_REFRESH_RATE, refresh_rate);

      // Ajustamos el valor del semáforo con nombre
      time_to_wait.tv_sec = time(NULL) + refresh_rate;
  
      // Leemos los valores se los sensores
      temperature = readSensor(TI, TAG_TEMP, nBytesTAG_TEMP, nBytesTEMP) / 100.00;
      usleep(50000);
      rain = readSensor(TI, TAG_RAIN, nBytesTAG_RAIN, nBytesRAIN);
      usleep(50000);
      humidity = readSensor(TI, TAG_HUM, nBytesTAG_HUM, nBytesHUM);

      // Obtenemos el timestamp
      timestamp = getTime();

      // Leemos el pronóstico del fichero
      buf = readVar(TAG_FORECAST);
      forecast = atoi((char *) &buf);

      // Convertimos los valores a char*
      sprintf(temperature_char, "%.2f", temperature);
      sprintf(rain_char, "%d", rain);
      sprintf(humidity_char, "%d", humidity);
      sprintf(forecast_char, "%d", forecast);

      // Creamos los char** que cotienen variables y vectores
      char * variables_array[] = {TAG_TIMESTAMP, TAG_TEMP, TAG_RAIN, TAG_HUM, TAG_FORECAST, NULL};
      char * values_array[] = {timestamp, temperature_char, rain_char, humidity_char, forecast_char, NULL};

      printf("%s: %s\n", TAG_TEMP, temperature_char);
      printf("%s: %s\n", TAG_RAIN, rain_char);
      printf("%s: %s\n", TAG_HUM, humidity_char);
      printf("%s: %s\n", TAG_FORECAST, forecast_char);
      printf("%s\n", timestamp);

      // Guardamos en el log los valores
      writeVar(variables_array, values_array);

      sem_timedwait(sem, &time_to_wait);
    }

    sem_close(sem);
    sem_unlink(sem_name);
  }

  return 0;
}
```

## Referencias

[[1]](http://linux.die.net/man/3/sem_timedwait) Instrucción sem_timedwait.
  
[[2]](http://linux.die.net/man/3/sem_getvalue) Instrucción sem_getvalue.
  
[[3]](http://linux.die.net/man/3/sem_open)[[4]](http://pubs.opengroup.org/onlinepubs/7908799/xsh/sem_open.html) Instrucción sem_open.
