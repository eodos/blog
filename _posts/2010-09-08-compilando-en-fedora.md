---
id: 181
title: Compilando en Fedora
date: 2010-09-08T01:11:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=4
permalink: /gnu-linux/compilando-en-fedora
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2010/09/compilando-en-fedora.html
blogger_author:
  - Eodos
views:
  - 76
categories:
  - GNU/Linux
tags:
  - fedora
---
# Preparamos el entorno de compilación

Instalamos el paquete kdelibs-devel, que contiene lo necesario para compiar C, C++...

`sudo yum install kdelibs-devel`

De estos no estoy muy seguro, ya que los instalé antes. En teoría no son necesarios, pero no perdéis nada por instalarlos si os da error la compilación

`$ sudo yum install gcc<br />
$ sudo yum install gcc-c++<br />
$ sudo yum install qt3-devel`

## Compilamos

``$ cd ruta_del_proyecto<br />
$ cd build<br />
$ sudo cmake -DCMAKE_INSTALL_PREFIX=`kde4-config --prefix` -DCMAKE_BUILD_TYPE=debugfull ..``

En caso de recibir el error No cmake\_minimum\_required command is present., debéis añadir **cmake\_minimum\_required(VERSION X.X)** (en mi caso 2.8) al inicio del archivo **CMakeLists.txt**, situado en la carpeta del proyecto.
  
Una vez se realice correctamente, escribimos:

`$ make`

## Instalamos

**Instalar para todos los usuarios**

`$ su -c 'make install'`

**Instalar únicamente al usuario actual**

`$ make install`

## Para desinstalar

`$ cd ruta_del_proyecto<br />
$ cd build`

Si está instalado en todos los usuarios

`$ su -c 'make uninstall'`

Instalado únicamente en el usuario actual

`$ make uninstall`