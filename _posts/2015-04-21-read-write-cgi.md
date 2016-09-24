---
id: 542
title: 'PFG - Scripts read.cgi y write.cgi'
date: 2015-04-21T18:37:55+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=542
permalink: /proyectos/read-write-cgi
categories:
  - Proyectos
tags:
  - c
  - cgi
  - json
  - pfg
  - Python
  - read
  - write
---
## Introducción

El primer paso es actualizar el compilador de C a la última versión, para ello ejecutamos en la terminal:

<pre class="lang:sh decode:true ">> pacman -Sy gcc gcc-libs</pre>

Y obtendremos la versión 4.9.2-2 del compilador (la más actualizada a fecha del proyecto).

Para simplificar la lectura y escritura de variables se han utilizado ficheros al estilo Linux. El nombre del fichero indica la variable que contiene y dentro únicamente aparece esta.

&nbsp;

## Scripts en Python

En una primera aproximación al problema, se han desarrollado los scripts en Python. Al ser un lenguaje de alto nivel cada script se puede escribir en menos de 20 líneas.

&nbsp;

### read.py

En primer lugar el script **read.py** le indica al sistema la ruta en la que se encuentra el binario de Python con el que ejecutar el script, en la línea 1. A continuación importa las librerías **cgi**, **cgitb** (para el debug), y **json** en las líneas 3, 4 y 5. En la línea 7 activar el depurador. En la línea 9 indica que va escribir contenido en **json**. A continuación carga las variables enviadas junto a la URL en una lista y mediante un bucle _for_ recorre la lista, lee el fichero adecuado y vuelca el valor en un _diccionario_. Por último transforma el diccionario en un objeto json y lo imprime.

<pre class="lang:python decode:true">#!/usr/bin/python2

import cgi
import cgitb
import json

cgitb.enable()

print "Content-Type: application/json\n\n"

data = cgi.FieldStorage()

dict = {}

for var in data.getlist("var"):
    f = open("vars/"+var,'r')
    dict[var]=f.read().rstrip('\n')
    f.close()

print json.dumps(dict)</pre>

&nbsp;

### write.py

Es un programa muy similar al anterior. Tras cargar los pares variable / valor en una lista, abre los archivos necesarios y reemplaza su contenido por el de la nueva variable.

<pre class="lang:python decode:true ">#!/usr/bin/python2

import cgi
import cgitb
import json

cgitb.enable()

print "Content-Type: application/json\n\n"

data = cgi.FieldStorage()

for var in data:
    f = open("vars/"+var,'w')
    f.write(data[var].value)
    f.close()

print "Success"</pre>

&nbsp;

## Scripts en C

Los scripts anteriores, pese a su facilidad de desarrollo, añaden el inconveniente de su lentitud de ejecución al tener que llamar al intérprete cada vez. El tiempo de ejecución del script **read.py** es de 2 segundos aproximadamente, mientras que si se realiza en **C** el resultado de obtiene de forma casi inmediata. Por tanto se considera que merece la pena emplear más tiempo en el desarrollo de una versión del programa desarrollada en C.

&nbsp;

### Preparativos

El script **read.cgi** hace necesario el uso de una librería que transforme nuestra salida a **objeto JSON** para que la aplicación Android sea capaz de procesarla. Se ha escogido la librería **json-builder** y **json-parser** disponibles en [github](https://github.com/udp/json-builder).

Para su instalación, primero instalamos **git** en la Raspberry Pi:

<pre class="lang:sh decode:true ">> pacman -Sy git perl-error</pre>

A continuación clonamos el repositorio **json-builder** y copiamos la librería y el header a la carpeta donde estamos desarrollando los scripts.

<pre class="lang:sh decode:true">> git clone http://github.com/udp/json-builder
> cd json-builder
> cp json-builder.c /srv/http/cgi-bin/json-builder.c
> cp json-builder.h /srv/http/cgi-bin/json-builder.h</pre>

Realizamos lo mismo con la librería **json-parser**.

<pre class="lang:sh decode:true">> git clone http://github.com/udp/json-parser
> cd json-parser
> cp json.c /srv/http/cgi-bin/json.c
> cp json.h /srv/http/cgi-bin/json.h</pre>

Es necesario modificar el archivo **json-builder.h** y cambiar la la llamada a la librería **<json.h>** a **&#8220;json.h&#8221;**. Pues en lugar de ser una librería del sistema es una librería que se encuentra en el directorio actual. Por defecto viene como librería del sistema puesto que se puede compilar para ser usada de esta manera. Pero debido a los múltiples errores obtenidos se realiza de esta forma.

Es importante además añadir el argumento **-lm** al compilar y realizar el **linkado** adecuado.

En vista al futuro uso de funciones compartidas se decide crear una librería que las aloje y un header que contenga los encabezados de dichas funciones y les asigne el tipo **extern** para que sean compartidas.

&nbsp;

### functions.c

De momento la única función que contiene el fichero es **char \* ruta(char \* variable)**, la cual crea la ruta del fichero que contiene la variable que se le pasa como argumento.

<pre class="lang:c decode:true ">#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "functions.h"

extern char * ruta(char * variable)
{
    char * temp;
    char * directorio = "vars/";
    if((temp = malloc(strlen(directorio)+strlen(variable)+1)) != NULL) {
        temp[0]='\0';
        strcat(temp, directorio);
        strcat(temp, variable);
        return temp;
    }
}</pre>

&nbsp;

### functions.h

Contiene el encabezado de las funciones contenidas en **functions.c**.

<pre class="lang:sh decode:true ">extern char * ruta(char * variable);</pre>

&nbsp;

## read.cgi

El script llama en primer lugar a las librerías utilizadas:

  * <stdio.h>
  * <stdlib.h>
  * &#8220;functions.h&#8221;
  * &#8220;json-builder.h&#8221;

A continuación reserva espacio en memoria para las variables que va a utilizar y guarda los parámetros pasados en la URL en una variable de tipo _(char *)_. A continuación separa los parámetros de la lista usando el método **strtok** usando como **token** el símbolo _&_. A continuación, para cada elemento de la lista separado y almacenado en un array char, accede al fichero correspondiente usando la función **ruta** para calcular el directorio en el que se encuentra y lo guarda en un array char usando la función **sscanf**. Crea el objeto json, añade cada par variable / valor leído, reserva espacio en memoria para el objeto y lo convierte.

<pre class="lang:c decode:true ">#include <stdio.h>
#include <stdlib.h>
#include "functions.h"
#include "json-builder.h"

int main() {

    FILE * f;
    char * data;
    char * pch[20];
    char * vars[20];
    char values[20];
    char * nombre_fichero;
    json_value * json_obj = json_object_new(0);

    printf("Content-Type: application/json\n\n");

    data = (char *) getenv("QUERY_STRING");

    if (data != NULL) {

        int i=0;
        pch[0] = (char *) strtok(data,"&");

        while (pch[i] != NULL) {
            i++;
            pch[i] = (char *) strtok(NULL,"&");
        }

        int nelements=i;

        for (i=0; i<nelements; i++) {
            sscanf(pch[i],"var=%s", &vars[i]);
            nombre_fichero = ruta((char *) &vars[i]);
            f = fopen((char *) nombre_fichero,"r");

            if (f != NULL) {
                fscanf(f, "%s", &values[i]);
                json_object_push(json_obj, (json_char *) &vars[i], json_string_new(&values[i]));
                fclose(f);
            }
        }

        char * buf = malloc(json_measure(json_obj));
        json_serialize(buf, json_obj);

        printf("%s\n", buf);
    }
}</pre>

Para compilarlo, ejecutamos:

<pre class="lang:sh decode:true ">> gcc -o read.cgi read.c functions.c json-builder.c -lm</pre>

&nbsp;

### write.cgi

Similar al programa anterior, lee la cadena enviada en la URL, la separa en variable / valor usando la función **sscanf** y escribe el valor en el fichero adecuado.

<pre class="lang:c decode:true ">#include <stdio.h>
#include <stdlib.h>
#include "functions.h"

int main() {

    FILE *f;
    char *data;
    char var[20];
    char * nombre_fichero;
    int value;

    printf("Content-Type: application/json\n\n");

    data = getenv("QUERY_STRING");

    if (data != NULL)
        sscanf(data,"%[^'=']=%d",&var,&value);

    nombre_fichero=ruta(var);

    f = fopen(nombre_fichero,"w");

    if (f != NULL) {
        fprintf(f, "%d",value);
        fclose(f);
        printf("Success");
    }
}</pre>

Para compilarlo, ejecutamos:

<pre class="lang:sh decode:true">gcc -o write.cgi write.c functions.c
</pre>

&nbsp;

## Referencias

  * [[1]](http://stackoverflow.com/questions/4872923/conversion-of-integer-pointer-to-integer) Cast pointer to integer.
  * [[2]](https://www.cs.tut.fi/~jkorpela/forms/cgic.html) Using query_string in C.
  * [[3]](http://www.cs.bu.edu/teaching/c/file-io/intro/) Manipulating files in C.
  * [[4]](http://c.conclase.net/librerias/?ansifun=fscanf) fscanf.
  * [[5]](http://stackoverflow.com/questions/15622409/how-to-link-multiple-implementation-files-in-c) Link multiple files in C.
  * [[6]](https://gpraveenkumar.wordpress.com/2009/06/10/how-to-use-scanf-to-read-string-with-space/) Usar scanf con delimitador.
  * [[7]](https://docs.python.org/2/library/cgi.html) Librería cgi de Python.
  * [[8]](https://docs.python.org/2/tutorial/datastructures.html?highlight=dictionary) Diccionarios en Python.
  * [[9]](https://docs.python.org/2/tutorial/inputoutput.html) Ficheros en Python.
  * [[10]](https://docs.python.org/2/library/string.html#string.rstrip) Strings en Python.
  * [[11]](https://docs.python.org/2/library/json.html) Librería JSON de Python.
  * [[12]](https://github.com/udp/json-builder) Librería json-builder de C.
  * [[13]](https://github.com/udp/json-parser) Librería json-parser de C.
