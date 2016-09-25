---
id: 606
title: 'PFG - Pronóstico meteorológico y creación de procesos'
date: 2015-05-11T19:39:01+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=606
permalink: /proyectos/pronostico-meteorologico
categories:
  - Proyectos
tags:
  - api
  - child
  - fork
  - pdf
  - proceso
  - procesos
  - Python
  - weather
  - yahoo
  - yql
---
## Introducción<a href="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yql.jpg" data-rel="lightbox-0" title=""><img class="  wp-image-613 alignright" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yql.jpg?resize=388%2C215" alt="yql" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yql.jpg?w=672&ssl=1 672w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yql.jpg?resize=300%2C166&ssl=1 300w" sizes="(max-width: 388px) 100vw, 388px" data-recalc-dims="1" /></a>

En la aplicación del riego automático es interesante contar con el pronóstico meteorológico de los próximos días para poder prever la caída de precipitaciones y por tanto dotar al sistema de una variable adicional a tener en cuenta al decidir cuándo poner en marcha sus actuadores.

Para dotar de esta capacidad al sistema se decide utilizar la **API** de **Yahoo**. Esta API permite un límite de 2000 peticiones al día y nos da el pronóstico meteorológico del día actual y de los siguientes tres. Información suficiente para un primer prototipo del sistema.

**Yahoo** utiliza un servicio llamado **YQL**, el cual permite a desarrolladores realizar búsquedas y peticiones en las distintas bases de datos de Yahoo.

En nuestro caso se harán peticiones a dos bases de datos:

  * Weather: para obtener el pronóstico meteorológico de un lugar conocido su **woeid** (un identificador que emplea Yahoo para referirse a cualquier propiedad del planeta Tierra).
  * Geo: nos permitirá obtener el **woeid** de una ciudad.

&nbsp;

## Petición YQL

Desde la **consola YQL** de Yahoo podemos realizar peticiones y observar la salida que producirían en formato XML o JSON.

En nuestro caso, si queremos en primer lugar obtener el código woeid de Málaga, podemos ejecutar (el nombre de la ciudad y el país deben estar en inglés):

<pre class="lang:sh decode:true ">select woeid from geo.places where text='malaga, spain'</pre>

Obteniendo la salida:

<pre class="lang:js decode:true">{
 "query": {
  "count": 10,
  "created": "2015-05-11T17:58:06Z",
  "lang": "en-US",
  "diagnostics": {
   "publiclyCallable": "true",
   "url": {
    ...
  },
  "results": {
   "place": [
    {
     "woeid": "766356"
    },
    {
     "woeid": "2444417"
    },

...</pre>

Observamos que obtenemos varios valores de woeid, de varias localizaciones cercanas a la ciudad de Málaga (Ciudad, Aeropuerto...). En nuestro caso nos vale con quedarnos con el primero, el general de Málaga.

Si ahora queremos obtener el pronóstico meteorológico para esta localización podemos anidar las peticiones:

<pre class="lang:sh decode:true ">select * from weather.forecast where woeid in (
  select woeid from geo.places where text='malaga, spain'
) and u='c'</pre>

Obteniendo:

<pre class="lang:java decode:true">{
 "query": {
  "count": 10,
  "created": "2015-05-11T18:01:01Z",
  "lang": "en-US",
  "diagnostics": {
   "publiclyCallable": "true",
   "url": [
    {
     ...
  },
  "results": {
   "channel": [
    {
     "title": "Yahoo! Weather - Malaga, ES",
     "link": "http://us.rd.yahoo.com/dailynews/rss/weather/Malaga__ES/*http://weather.yahoo.com/forecast/SPXX0052_c.html",
     "description": "Yahoo! Weather for Malaga, ES",
     "language": "en-us",
     "lastBuildDate": "Mon, 11 May 2015 7:30 pm CEST",
     "ttl": "60",
     "location": {
      "city": "Malaga",
      "country": "Spain",
      "region": ""
     },
     "units": {
      "distance": "km",
      "pressure": "mb",
      "speed": "km/h",
      "temperature": "C"
     },
     "wind": {
      "chill": "22",
      "direction": "130",
      "speed": "24.14"
     },
     "atmosphere": {
      "humidity": "60",
      "pressure": "1015.92",
      "rising": "0",
      "visibility": "9.99"
     },
     "astronomy": {
      "sunrise": "7:14 am",
      "sunset": "9:12 pm"
     },
     "image": {
      "title": "Yahoo! Weather",
      "width": "142",
      "height": "18",
      "link": "http://weather.yahoo.com",
      "url": "http://l.yimg.com/a/i/brand/purplelogo//uh/us/news-wea.gif"
     },
     "item": {
      "title": "Conditions for Malaga, ES at 7:30 pm CEST",
      "lat": "36.72",
      "long": "-4.44",
      "link": "http://us.rd.yahoo.com/dailynews/rss/weather/Malaga__ES/*http://weather.yahoo.com/forecast/SPXX0052_c.html",
      "pubDate": "Mon, 11 May 2015 7:30 pm CEST",
      "condition": {
       "code": "34",
       "date": "Mon, 11 May 2015 7:30 pm CEST",
       "temp": "22",
       "text": "Fair"
      },
      "description": "\n<img src=\"http://l.yimg.com/a/i/us/we/52/34.gif\"/><br />\n<b>Current Conditions:</b><br />\nFair, 22 C<BR />\n<BR /><b>Forecast:</b><BR />\nMon - Clear. High: 25 Low: 16<br />\nTue - Sunny. High: 26 Low: 17<br />\nWed - Mostly Sunny. High: 28 Low: 19<br />\nThu - Mostly Sunny. High: 33 Low: 22<br />\nFri - Sunny. High: 26 Low: 16<br />\n<br />\n<a href=\"http://us.rd.yahoo.com/dailynews/rss/weather/Malaga__ES/*http://weather.yahoo.com/forecast/SPXX0052_c.html\">Full Forecast at Yahoo! Weather</a><BR/><BR/>\n(provided by <a href=\"http://www.weather.com\" >The Weather Channel</a>)<br/>\n",
      "forecast": [
       {
        "code": "31",
        "date": "11 May 2015",
        "day": "Mon",
        "high": "25",
        "low": "16",
        "text": "Clear"
       },
       {
        "code": "32",
        "date": "12 May 2015",
        "day": "Tue",
        "high": "26",
        "low": "17",
        "text": "Sunny"
       },
       {
        "code": "34",
        "date": "13 May 2015",
        "day": "Wed",
        "high": "28",
        "low": "19",
        "text": "Mostly Sunny"
       },
       {
        "code": "34",
        "date": "14 May 2015",
        "day": "Thu",
        "high": "33",
        "low": "22",
        "text": "Mostly Sunny"
       },
       {
        "code": "32",
        "date": "15 May 2015",
        "day": "Fri",
        "high": "26",
        "low": "16",
        "text": "Sunny"
       }
      ],
      "guid": {
       "isPermaLink": "false",
       "content": "SPXX0052_2015_05_15_7_00_CEST"
      }
     }
    },
    
...</pre>

El valor que más nos interesa para saber si va a llover o no es  el **code** ubicado en el nodo **forecast**. Podemos filtrar la petición y quedarnos con los 4 primeros valores, correspondientes a los 4 días de predicción de la ciudad de Málaga y descartar el resto (correspondiente a otros woeid).

<pre class="lang:sh decode:true">select item.forecast.code from weather.forecast where woeid in (
  select woeid from geo.places where text='Malaga, Spain'
) and u='c'</pre>

Nos devuelve:

<pre class="lang:java decode:true ">{
 "query": {
  "count": 40,
  "created": "2015-05-11T18:08:49Z",
  "lang": "en-US",
  "diagnostics": {
   "publiclyCallable": "true",
   "url": [
    {
     ...
  },
  "results": {
   "channel": [
    {
     "item": {
      "forecast": {
       "code": "31"
      }
     }
    },
    {
     "item": {
      "forecast": {
       "code": "32"
      }
     }
    },
    {
     "item": {
      "forecast": {
       "code": "34"
      }
     }
    },
    {
     "item": {
      "forecast": {
       "code": "34"
      }
     }
    },

...</pre>

Como observamos en la siguiente tabla, los códigos indican una descripción del tiempo que va a hacer ese día.

<a href="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yahoo.png" data-rel="lightbox-1" title=""><img class=" size-full wp-image-607 aligncenter" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yahoo.png?resize=534%2C994" alt="yahoo" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yahoo.png?w=534&ssl=1 534w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/05/yahoo.png?resize=161%2C300&ssl=1 161w" sizes="(max-width: 534px) 100vw, 534px" data-recalc-dims="1" /></a>

&nbsp;

Observamos que si el código está entre 19 y 34 o vale 46 o 44 indica que ese día no va a llover.

&nbsp;

## Script en Python para la petición

Se ha escogido realizar este script en Python al permitir trabajar fácilmente con objetos JSON, peticiones web y lecturas / escrituras a ficheros. Además de que el script se ejecutará de forma independiente al hilo principal de ejecución debido a que los pronósticos no es necesario obtenerlos con tanta frecuencia como las lecturas de los sensores.

Creamos por tanto un script que esté en un bucle infinito y lea en un fichero la tasa con la que debe realizar las peticiones (fichero _/vars/FORECAST_RATE_). A continuación leerá la localización de la Raspberry Pi del fichero _vars/LOCATION_. Esto permitirá al usuario del sistema cambiar la localización fácilmente desde la aplicación Android y obtener los pronósticos actualizados para esa localización.

A continuación forma la URL para acceder a la API y realiza la petición con las librerías **urllib** y **urllib2**. Además indica que quiere recibir la respuesta en formato JSON.

Para realizar la petición se ha incluido un bloque **try**, lo que nos permitirá saber si ha habido un error al realizar la lectura (si, por ejemplo, el servicio de Yahoo está caído o no tenemos conexión a internet), y por tanto la Raspberry Pi tendrá en cuenta que el valor que está devolviendo este script no es válido.

Una vez realizada la petición y transformado el objeto JSON recorremos los campos que nos interesan y vemos si el código está en el intervalo que nos interesa. Si no lo está (hay lluvia anunciada) se anota en un fichero de salida (_vars/FORECAST_) que el día _D_ va a llover. Si ninguno de los 4 días va a llover, anota 4 en el fichero. Si no ha podido realizar la petición, escribe -1.

A continuación el script duerme el tiempo indicado (en minutos) y vuelve a ejecutarse entero.

<pre class="lang:python decode:true ">#!/usr/bin/python2

# Este programa realiza una peticipn a la API de Yahoo Weather para obtener el pronostico del tiempo de 4 dias y analiza si alguno de ellos
# indica lluvia. En caso afirmativo almacena el numero del dia en el fichero. En caso negativo almacena 4. Si no puede conectar al servidor 
# escribe -1. Obtiene la localizacion de la cual buscar el pronostico del fichero vars/LOCATION. Debe estar en formato "Ciudad, Pais". 
# Guarda la salida en vars/FORECAST.

import urllib2, urllib, json
from time import sleep

while True:

    forecast = [0,0,0,0]

    # Si ninguno de los 4 días de pronóstico va a llover, almacenamos un 4
    next_rain = 4

    # Leemos el tiempo de espera entre dos peticiones para obtener el pronostico, en minutos
    f = open("vars/FORECAST_RATE", 'r')
    rate = f.read()
    f.close()

    # Leemos la localizacion de la cual obtener el pronostico
    f = open("vars/LOCATION", 'r')
    location = f.read()
    f.close()

    # Creamos la URL que contiene la peticion
    baseurl = "https://query.yahooapis.com/v1/public/yql?"
    yql_query = "select item.forecast.code from weather.forecast where woeid in (select woeid from geo.places where text='" + location + "') and u='c'"
    yql_url = baseurl + urllib.urlencode({'q':yql_query}) + "&format=json"

    # Realizamos la peticion a la API de Yahoo Weather
    try:
        result = urllib2.urlopen(yql_url).read()
        data = json.loads(result)
        for i in range(0, 4):

            # Obtenemos el codigo que indica el pronostico meteorologico de cada dia
            forecast[i] = data['query']['results']['channel'][i]['item']['forecast']['code']

            # Analizamos si ese codigo indica lluvia. Si es asi anotamos el dia en el que va a llover y pasamos a la escritura de la variable
            if not (int(forecast[i]) >= 19 and int(forecast[i]) <= 34) or (int(forecast[i]) == 36) or (int(forecast[i]) == 44):
                next_rain = i
                break

    # Si no podemos conectar a la API, escribimos -1 en el fichero salida
    except:
        next_rain = -1

    f = open("vars/FORECAST", 'w')
    f.write(str(next_rain))
    f.close()

    sleep(int(rate)*60)</pre>

Damos permisos de ejecución al fichero con:

<pre class="lang:sh decode:true">> chmod +x weather_forecast.py</pre>

Añadiendo algunas salidas por pantalla ejecutamos y obtenemos:

<a href="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/weather_python.png" data-rel="lightbox-2" title=""><img class=" size-full wp-image-608 aligncenter" src="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/weather_python.png?resize=465%2C263" alt="weather_python" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/weather_python.png?w=465&ssl=1 465w, https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/weather_python.png?resize=300%2C170&ssl=1 300w" sizes="(max-width: 465px) 100vw, 465px" data-recalc-dims="1" /></a>

&nbsp;

## Programa main.c

Se modifica el programa que contiene el hilo principal de ejecución para que mediante las instrucciones **fork()** y **execlp** cree un proceso hijo que ejecute el script de petición del pronóstico meteorológico. Para ello se añade la librería **<unistd.h>**.

Se añaden también las instrucciones de lectura de la salida del script y su almacenamiento en el fichero LOG.

<pre class="lang:c decode:true ">#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
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

    buf = readVar(TAG_REFRESH_RATE);
    refresh_rate = atoi((char *) &buf);
    printf("%s: %d\n", TAG_REFRESH_RATE, refresh_rate);

    // Creamos el proceso hijo y le indicamos que ejecute el script para obtener la predicción meteorológica
    pid = fork();

    if (pid == 0) {
        execlp("python", "python", "weather_forecast.py", NULL);
        exit(0);
    }

    // Proceso padre
    else {

        for(;;) {
        
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

            usleep(refresh_rate * 1000000);
        }
    }

    return 0;
}</pre>

Ejecutando podemos ver que en cuatro días no va a llover (salida -1):

<a href="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/c_weather.png" data-rel="lightbox-3" title=""><img class=" size-full wp-image-609 aligncenter" src="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/c_weather.png?resize=371%2C196" alt="c_weather" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/c_weather.png?w=371&ssl=1 371w, https://i0.wp.com/eodos.net/wp-content/uploads/2015/05/c_weather.png?resize=300%2C158&ssl=1 300w" sizes="(max-width: 371px) 100vw, 371px" data-recalc-dims="1" /></a>

Observamos que el fichero _vars/FORECAST_ se actualiza correctamente cada minuto (valor indicado en _vars/FORECAST_RATE_) y el log se actualiza correctamente:

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/log_weather.png" data-rel="lightbox-4" title=""><img class=" size-full wp-image-610 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/log_weather.png?resize=348%2C357" alt="log_weather" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/log_weather.png?w=348&ssl=1 348w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/05/log_weather.png?resize=292%2C300&ssl=1 292w" sizes="(max-width: 348px) 100vw, 348px" data-recalc-dims="1" /></a>

&nbsp;

## Referencias

[[1]](https://developer.yahoo.com/yql) Yahoo YQL.
  
[[2]](https://developer.yahoo.com/yql/console/) Consola YQL de Yahoo.
  
[[3]](https://developer.yahoo.com/weather/documentation.html) Documentación sobre los pronósticos meteorológicos que da la API de Yahoo.
  
[[4]](http://linux.die.net/man/3/execlp) Comando execlp.
  
<a href="http://freshclickmedia.co.uk/wp-content/uploads/2013/11/yql.jpg" data-rel="lightbox-5" title="">[5]</a> Imagen YQL utilizada.

&nbsp;
