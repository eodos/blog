---
id: 261
title: Instalando Skype en Fedora x86_64
date: 2012-12-16T23:43:43+00:00
author: eodos
layout: post
guid: http://blog.eodos.net/?p=261
permalink: /gnu-linux/instalando-skype
categories:
  - GNU/Linux
tags:
  - fedora
  - librerias
  - Linux
  - Skype
  - x64
  - x86_64
---
<p style="text-align: justify;">
  <a href="https://i0.wp.com/eodos.net/wp-content/uploads/2012/12/fedora-81.jpg" data-rel="lightbox-0" title=""><img class="size-medium wp-image-262 aligncenter" src="https://i2.wp.com/eodos.net/wp-content/uploads/2012/12/fedora-81-300x137.jpg?resize=300%2C137" alt="Fedora" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2012/12/fedora-81.jpg?resize=300%2C137&ssl=1 300w, https://i0.wp.com/eodos.net/wp-content/uploads/2012/12/fedora-81.jpg?w=538&ssl=1 538w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>Para instalar <strong>Skype</strong> en Fedora, podemos bajar el paquete rpm desde la <a href="http://www.skype.com/intl/es/get-skype/on-your-computer/linux/" target="_blank">web oficial</a> o desde los repositorios de Fedora. En este segundo caso, basta con abrir la <strong>Terminal </strong>e introducir:
</p>

<pre class="">$ sudo yum install skype</pre>

<p style="text-align: justify;">
  Pero el problema llega a la hora de abrirlo, ya que nos aparecerá el siguiente error:
</p>

<pre class="">$ skype
skype: error while loading shared libraries: libXss.so.1: cannot open
shared object file: No such file or directory</pre>

<p style="text-align: justify;">
  Nos hace falta la librería de 32 bits <strong>libXss.so</strong>, ya que la versión de Skype para Linux que hemos instalado es de 32 bits. No nos sirve copiar la librería correspondiente de 64 bits, por tanto buscamos en qué paquete podemos encontrarla.
</p>

<pre class="">$ sudo yum search libXss
Loaded plugins: langpacks, presto, refresh-packagekit
========= N/S Matched: libXss =========
libXScrnSaver.i686 : X.Org X11 libXss runtime library
libXScrnSaver.x86_64 : X.Org X11 libXss runtime library</pre>

<p style="text-align: justify;">
  Instalamos la librería que falta:
</p>

<pre class="">$ sudo yum install libXScrnSaver.i686</pre>

<p style="text-align: justify;">
  Volvemos a intentar ejecutar skype, obteniendo un nuevo error.
</p>

<pre class="">$ skype 
 skype: error while loading shared libraries: libQtDBus.so.4:
 cannot open shared object file: No such file or directory</pre>

<p style="text-align: justify;">
  Vuelve a faltarnos una librería, en este caso <strong>libQtDBus<em>. </em></strong>Vamos a buscar en nuestro equipo la que tenemos instalada (será la de 64 bits), veremos a qué paquete pertenece con el comando <strong>rpm -qf</strong> e instalaremos el correspondiente paquete de 32 bits.
</p>

<pre class="">$ locate libQtDBus
 /usr/lib64/libQtDBus.prl
 /usr/lib64/libQtDBus.so
 /usr/lib64/libQtDBus.so.4
 /usr/lib64/libQtDBus.so.4.8
 /usr/lib64/libQtDBus.so.4.8.0
 /usr/lib64/libQtDBus_debug.so

$ rpm -qf /usr/lib64/libQtDBus.prl
 qt-devel-4.8.0-7.fc16.x86_64

$ sudo yum search qt-devel
 ...
 qt-devel.i686 : Development files for the Qt toolkit
 qt-devel.x86_64 : Development files for the Qt toolkit
 ...

$ sudo yum install qt-devel.i686</pre>

<p style="text-align: justify;">
  Ahora podemos abrir <strong>skype</strong> sin errores. Si volvéis a tener un error parecido, buscad el correspondiente paquete de 32 bits y listo.
</p>

<p style="text-align: justify;">