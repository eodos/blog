---
id: 456
title: 'PFG - Control remoto de la Raspberry PI mediante CGI'
date: 2014-11-14T23:55:02+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=456
permalink: /proyectos/raspberry-cgi
categories:
  - Proyectos
tags:
  - apache
  - arduino
  - cgi
  - i2c
  - permisos
  - raspberry
---
<p style="text-align: justify;">
  <strong>CGI</strong> es una tecnología que permite a un cliente solicitar datos de un programa ejecutado en un servidor web. Nos va a ser útil al permitirnos ejecutar programas alojados en la Raspberry Pi desde un navegador e incluso desde un terminal Android.
</p>

<p style="text-align: justify;">
  Para ello, en primer lugar instalamos<strong> Apache </strong>en la Raspberry Pi
</p>

<pre class="lang:sh decode:true">> pacman -S apache</pre>

<p style="text-align: justify;">
   y hacemos que su <em>demonio </em>se ejecute automáticamente al iniciar el equipo
</p>

<pre class="lang:sh decode:true">> systemctl enable httpd</pre>

<h1 style="text-align: justify;">
  Configuración de Apache
</h1>

<p style="text-align: justify;">
  Vamos a repasar las líneas más significativas del fichero de configuración por defecto de Apache, localizado en <em>/etc/httpd/conf/httpd.conf</em>
</p>

<pre class="wrap:true lang:sh mark:7,8 decode:true">ServerRoot "/etc/httpd" 
#Los ficheros de configuración se almacenan en este directorio.

Listen 80 
#El servidor escuchará las peticiones que reciba por el puerto 80. Todo correcto.

LoadModule cgi_module modules/mod_cgi.so 
#Es muy importante descomentar esta línea. En caso contrario los script CGI que escribamos no se podrán ejecutar.

User http
Group http
# El usuario que atenderá y resolverá las peticiones que reciba Apache. Nos afectará más adelante.

<Directory />
    AllowOverride none
    Require all denied
</Directory>
# Todas las peticiones recibidas se deniegan por defecto, excepto para los casos definidos a continuación.

DocumentRoot "/srv/http"
<Directory "/srv/http">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
# Fijamos el directorio principal a la carpeta /srv/http, lo cual significa que cuando nos dirijamos a http://domain.com/index.html estaremos abriendo el archivo index.html ubicado en esa carpeta. Además se le indica a Apache que atienda las peticiones que lleguen a esa ruta.

ScriptAlias /cgi-bin/ "/srv/http/cgi-bin/"
#Esta línea establece un alias a la carpeta donde almacenaremos los scripts en CGI y les da permisos de ejecución, necesarios para funcionar.

<Directory "/srv/http/cgi-bin">
    Options None
    AllowOverride None
    Require all granted
</Directory>
# De nuevo, damos la orden de atender las peticiones llegadas a este directorio.</pre>

<p style="text-align: justify;">
   Es importante que los archivos CGI que queramos que se ejecuten tengan permisos de ejecución para el usuario <em>http</em>, y además la ruta en la que se encuentre también tenga permisos de ejecución. En mi caso ha sido necesario darle permisos al directorio raíz /, en caso contrario al ejecutar un script recibía un error <strong>403 forbidden</strong>.
</p>

<p style="text-align: justify;">
  A continuación realizamos la prueba escribiendo un pequeño script CGI en C que muestre un texto en el navegador.
</p>

<pre class="lang:c decode:true">#include <stdio.h>
int main() {
    printf("Content-Type: text/html\n\n");
    printf("Hello world");
    return 0;
}</pre>

<p style="text-align: justify;">
   Lo compilamos con <b>gcc</b>, le damos permisos de ejecución y vemos el resultado
</p>

<img class="aligncenter wp-image-473 size-full" src="https://i2.wp.com/eodos.net/wp-content/uploads/2014/11/first.png?resize=453%2C116" alt="first" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2014/11/first.png?w=453&ssl=1 453w, https://i2.wp.com/eodos.net/wp-content/uploads/2014/11/first.png?resize=300%2C76&ssl=1 300w" sizes="(max-width: 453px) 100vw, 453px" data-recalc-dims="1" />

<p style="text-align: justify;">
  Funciona correctamente, por lo que podemos pasar al siguiente nivel.
</p>

<h1 style="text-align: justify;">
  Adaptando nuestro programa i2c.c para ser ejecutado como script CGI
</h1>

<p style="text-align: justify;">
  Para que nuestro programa se ejecute correctamente sin dar errores es necesario añadir los <strong>headers</strong> necesarios para que sea interpretado como html y los printf que realizamos aparezcan en pantalla. Además, se le ha añadido un título de página y se han adaptado los saltos de línea a html. El programa queda así:
</p>

<pre class="lang:c decode:true  ">#include <stdio.h>
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
    
    printf("Content-Type: text/html\n\n");
    printf("<html>");
    printf("<head><title>Test</title></head");
    printf("<body>");

    
    printf("I2C: Conectando...<br/>");
    if ((file = open("/dev/i2c-0", O_RDWR)) < 0) {
        printf("I2C: error de acceso");
        exit(1);
    }
    
    printf("I2C: Accediendo al dispositivo<br/>");
    if (ioctl(file, I2C_SLAVE, ADDRESS) < 0) {
        printf("I2C: error al acceder al esclavo en 0x%x", ADDRESS);
        exit(1);
    }
    
    printf("Enviado %d<br/>", enviado);
    cmd[0] = enviado;
    if (write(file, cmd, 1) == 1) {
        usleep(10000);
        if (read(file, buff, 1) == 1) {
            recibido = (int) buff[0];
            printf("Recibido %d<br/>", recibido);
        }
    }
    
    printf("I2C: Terminando...");
    printf("</html>");
    close(file);
    return (EXIT_SUCCESS);
}
</pre>

<p style="text-align: justify;">
   Compilamos con <strong>gcc:</strong>
</p>

<pre class="lang:sh decode:true ">> gcc i2c.c -o i2c.cgi</pre>

<p style="text-align: justify;">
   Sin embargo, al ejecutarlo obtenemos el siguiente error:
</p>

<img class="aligncenter wp-image-475 size-full" src="https://i2.wp.com/eodos.net/wp-content/uploads/2014/11/error.png?resize=425%2C135" alt="error" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2014/11/error.png?w=425&ssl=1 425w, https://i2.wp.com/eodos.net/wp-content/uploads/2014/11/error.png?resize=300%2C95&ssl=1 300w" sizes="(max-width: 425px) 100vw, 425px" data-recalc-dims="1" />

<p style="text-align: justify;">
  Nuestro programa no puede escribir sobre el dispositivo <strong>/dev/i2c-0</strong>, y es debido a que el script lo ejecuta el usuario <strong>http</strong> cuando realizamos la petición, y no tiene privilegios sobre ese dispositivo. Vamos a solucionarlo creando un grupo llamado <strong>i2c, </strong>añadiendo a dicho grupo los usuarios <em>root</em> y <em>http, </em>y dando permisos de lectura y escritura del fichero <strong>/dev/i2c-0</strong> al grupo.
</p>

<p style="text-align: justify;">
  Primero creamos el grupo y añadimos los usuarios:
</p>

<pre class="lang:sh decode:true ">> groupadd i2c
> usermod -a -G i2c http
> usermod -a -G i2c root</pre>

<p style="text-align: justify;">
   Modificamos el dueño de la carpeta donde se encuentran los scripts CGI al nuevo grupo:
</p>

<pre class="lang:sh decode:true ">> chown root:i2c -R /srv/http/cgi-bin/</pre>

<p style="text-align: justify;">
   Y por último modificamos los permisos del dispositivo. Estos permisos los controla el programa <strong>udev</strong>, y para hacerlos persistentes debemos añadir una nueva regla al programa, creando el fichero <em>/lib/udev/rules.d/60-i2c-tools.rules</em> con el contenido:
</p>

<pre class="lang:sh decode:true ">KERNEL=="i2c-0", GROUP="i2c", MODE="0660"</pre>

Ejecutando ahora:

<img class="aligncenter wp-image-476 size-full" src="https://i1.wp.com/eodos.net/wp-content/uploads/2014/11/success.png?resize=457%2C186" alt="success" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2014/11/success.png?w=457&ssl=1 457w, https://i1.wp.com/eodos.net/wp-content/uploads/2014/11/success.png?resize=300%2C122&ssl=1 300w" sizes="(max-width: 457px) 100vw, 457px" data-recalc-dims="1" />

El LED integrado en la placa Arduino Mega se enciende y apaga correctamente.