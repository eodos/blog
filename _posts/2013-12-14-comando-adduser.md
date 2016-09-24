---
id: 288
title: Linux. Comando adduser
date: 2013-12-14T20:57:51+00:00
author: eodos
layout: post
guid: http://blog.eodos.net/?p=288
permalink: /gnu-linux/comando-adduser
categories:
  - GNU/Linux
tags:
  - adduser
  - comandos
  - gnu
  - gnu/linux
  - Linux
---
<p style="text-align: justify;">
  Para añadir un nuevo usuario a nuestro sistema, usamos el comando <strong>adduser</strong> o <strong>useradd</strong>. Vamos a describir su uso y exponer algunos ejemplos:
</p>

<pre class="lang:sh decode:true">$ sudo adduser <usuario></pre>

<p style="text-align: justify;">
  De esta forma se crea un nuevo usuario de nombre <<em>usuario</em>>, con todas las opciones por defecto, tales como ubicación de su directorio, duración de la cuenta de usuario, shell a utilizar o grupos en los que va a ser incluído. Estos parámetros se pueden cambiar más adelante usando el comando <strong>usermod</strong>. Este comando admite las mismas opciones que el comando <strong>adduser</strong>, que son:
</p>

<ul style="text-align: justify;">
  <li>
    <strong>-c</strong> &#8216;<<em>comentario</em>>&#8217;. Permite añadir un comentario al usuario, como puede ser su nombre real.
  </li>
  <li>
    <strong>-d</strong> <<em>directorio</em>>. Esta opción nos permite cambiar el directorio por defecto del usuario, que suele ser <strong>/home/<usuario></strong>.
  </li>
  <li>
    <strong>-e </strong><<em>YYYYMMDD</em>>. Permite seleccionar la fecha en la que la cuenta se deshabilitará. Debe introducirse en el formato indicado: añomesdía.
  </li>
  <li>
    <strong>-f </strong><días>. Nos permite seleccionar el tiempo en días a partir de la fecha de expiración de la contraseña en la cual la cuenta se deshabilitará. Con un valor de <strong>-1</strong>, no lo hará.
  </li>
  <li>
    <strong>-g </strong><<em>grupo</em>>. Permite añadir el usuario a un grupo. Debe existir con anterioridad para poder añadirlo. Podemos introducir el grupo por su nombre o por su ID.
  </li>
  <li>
    <strong>-G </strong><<em>grupos</em>>. Similar a la opción anterior, pero permite introducir varios grupos separados por comas.
  </li>
  <li>
    <strong>-m</strong>. Crea el directorio del usuario si no existe.
  </li>
  <li>
    <strong>-M</strong>. No crea el directorio del usuario.
  </li>
  <li>
    <strong>-n</strong>. No crea un grupo privado para el usuario.
  </li>
  <li>
    <strong>-r</strong>. La cuenta se convierte en cuenta del sistema, con ID de usuario (<strong>UID</strong>) menor a 500 y sin directorio.
  </li>
  <li>
    <strong>-p </strong><<em>contraseña</em>>. Establece una contraseña de usuario. Se puede crear posteriormente con el comando <strong>passwd </strong><<em>usuario</em>>. Se encriptará con <strong>crypt</strong>.
  </li>
  <li>
    <strong>-s </strong><<em>shell</em>>. Permite modificar la shell de inicio de sesión del usuario, por defecto <strong>/bin/bash</strong>.
  </li>
  <li>
    <strong>-u </strong><<em>UID</em>>. Nos permite especificar la ID del usuario, debe ser mayor a 499 y única.
  </li>
</ul>

<p style="text-align: justify;">
  Para ver todas las opciones, puedes usar el comando de ayuda <strong>man adduser</strong>.
</p>

<p style="text-align: justify;">
  <strong>Ejemplos:</strong>
</p>

<p style="text-align: justify;">
   Creamos un nuevo usuario, añadiendo un comentario.
</p>

<pre class="lang:sh decode:true">$ sudo adduser -c "David Paul, eodos0@gmail.com" eodos</pre>

<p style="text-align: justify;">
  Cambiamos el directorio por defecto, al ser un usuario propietario de una carpeta en el servidor de Apache:
</p>

<pre class="">$ sudo usermod -d /var/www/html/eodos eodos</pre>

<p style="text-align: justify;">
  Le cambiamos el shell a &#8220;/bin/ksh&#8221;
</p>

<pre class="">$ sudo usermod -s /bin/ksh eodos</pre>