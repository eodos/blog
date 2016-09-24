---
id: 31
title: ¿Qué significan las carpetas de Linux?
date: 2009-07-23T23:55:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=31
permalink: /gnu-linux/que-significan-las-carpetas-de-linux
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/07/que-significan-las-carpetas-de-linux.html
blogger_author:
  - Eodos
views:
  - 16
categories:
  - GNU/Linux
tags:
  - Curiosidades
  - Linux
---
¿Sabias que significa cada una de las carpetas en un sistema de archivos Linux?. A diferencia de Windows, en Linux los archivos y carpetas se organizan de manera diferente, en Windows cuando instalamos un programa se crea una carpeta donde estarán todos los archivos del programas, en Linux en cambio, la idea es agrupar en carpetas archivos con fines similares, es decir en un sectores todos los ejecutables, en otro las librerías, en otro las configuraciones, etc.

* /: Esta es la raíz.  
* /usr: Aquí se encuentra la gran mayoría de los archivos existentes en un sistema Linux, como documentación, ejecutable, etc.  
* /bin: Aquí están los comandos que pueden usar todos los usuarios (incluido el root).  
* /sbin: Aquí están los comandos que sólo puede usar el root.  
* /dev: Aqui están todos los dispositivos de nuestra máquina.  
* /home: Lugar donde se almacenan las cuentas de usuarios. Algo parecido a como es “mis documentos” en Windows.  
* /lib: Aquí están las librerías que se necesitan para el sistema.  
* /var: Contiene información variable, como por ejemplo los logs del sistema (/var/log), correo local, etc.  
* /tmp: Directorio temporal.  
* /etc: Aquí se encuentran todas las configuraciones. Por ejemplo si queremos modificar la configuración de Samba tan solo hay que editar el archivo de texto /etc/samba/smb.conf  
* /root: Cuenta del administrador.  
* /boot: Aquí está todo lo relacionado con el arranque del sistema.  
* /media: Punto de montaje para sistemas de archivos montados localmente.  
* /mnt: Es el predecesor de /media, se lo conserva solo por razones históricas  
* /proc: Sistema de archivos virtual de información de procesos y del kernel.