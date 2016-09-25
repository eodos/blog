---
id: 36
title: 'Compartir impresora. Windows -> Linux'
date: 2009-06-19T23:04:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=36
permalink: /gnu-linux/compartir-impresora-windows-linux
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/06/compartir-impresora-windows-linux.html
blogger_author:
  - Eodos
views:
  - 392
categories:
  - GNU/Linux
  - Windows
tags:
  - Linux
  - Windows
---
2º Parte del tutorial, en esta veremos cómo hacer el proceso inverso, compartir una impresora hospedada en Windows y poder usarla en Linux.

<span style="font-weight:bold;">Windows</span>

Inicio -> Panel de Control -> Conexiones de red

Click en Configurar una red doméstica (menú de la izquierda).  
Seleccionamos la opción "<span style="font-weight:bold;">Este equipo se conecta a internet a través de una puerta de enlace residencial o de otro equipo en mi red".<br /></span>  
<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i0.wp.com/2.bp.blogspot.com/_H4ctsPRjMs8/SjwAuWSe8OI/AAAAAAAAAIg/FrbxIqFN0xg/s1600-h/compartir2.JPG" data-rel="lightbox-0" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 300px;" src="https://i2.wp.com/2.bp.blogspot.com/_H4ctsPRjMs8/SjwAuWSe8OI/AAAAAAAAAIg/FrbxIqFN0xg/s400/compartir2.JPG" border="0" alt="" id="BLOGGER_PHOTO_ID_5349151253666394338" data-recalc-dims="1" /></a>

Damos a siguiente  
Escribimos un nombre para el equipo.

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i2.wp.com/2.bp.blogspot.com/_H4ctsPRjMs8/SjwAuQfCC9I/AAAAAAAAAIo/dzMCZLTCU9I/s1600-h/compartir3.JPG" data-rel="lightbox-1" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 300px;" src="https://i2.wp.com/2.bp.blogspot.com/_H4ctsPRjMs8/SjwAuQfCC9I/AAAAAAAAAIo/dzMCZLTCU9I/s400/compartir3.JPG" border="0" alt="" id="BLOGGER_PHOTO_ID_5349151252108413906" data-recalc-dims="1" /></a>

Damos a siguiente.  
Escogemos un nombre para el grupo de trabajo

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i1.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjwAuoqreMI/AAAAAAAAAIw/7ffXWVGARgM/s1600-h/compartir4.JPG" data-rel="lightbox-2" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 300px;" src="https://i2.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjwAuoqreMI/AAAAAAAAAIw/7ffXWVGARgM/s400/compartir4.JPG" border="0" alt="" id="BLOGGER_PHOTO_ID_5349151258599717058" data-recalc-dims="1" /></a>

Ya tenemos creado el grupo de trabajo, ahora a la impresora.

Inicio -> Panel de Control -> Impresoras  
Click derecho en la impresora a compartir, vamos a la pestaña Compartir y seleccionamos la opción: <span style="font-weight:bold;">"Compartir esta impresora"</span>.

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i2.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjwAuABJBQI/AAAAAAAAAIY/FCT7lGW6ob0/s1600-h/compartir1.JPG" data-rel="lightbox-3" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 300px;" src="https://i0.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjwAuABJBQI/AAAAAAAAAIY/FCT7lGW6ob0/s400/compartir1.JPG" border="0" alt="" id="BLOGGER_PHOTO_ID_5349151247688074498" data-recalc-dims="1" /></a>

<span style="font-weight:bold;">Linux</span>

Menu K -> Preferencias del Sistema -> Avanzado -> Configuración de impresora  
Click en <span style="font-weight:bold;">New Printer</span> (Nueva impresora).

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i1.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjwAu1dS2WI/AAAAAAAAAI4/g67urb5AwlU/s1600-h/instant%C3%A1nea1.png" data-rel="lightbox-4" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 300px;" src="https://i1.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjwAu1dS2WI/AAAAAAAAAI4/g67urb5AwlU/s400/instant%C3%A1nea1.png" border="0" alt="" id="BLOGGER_PHOTO_ID_5349151262033238370" data-recalc-dims="1" /></a>

Escogemos <span style="font-weight:bold;">"New Network Printer"</span>  
Escogemos "<span style="font-weight:bold;">Windows Printer via SAMBA"</span>

En la dirección escribimos:

<span style="font-weight:bold;">smb://grupo_trabajo/ordenador/impresora</span>

Por ejemplo en mi caso:

<span style="font-weight:bold;">smb://CASA/192.168.2.100/DX</span>

En vez de poner la IP interna, podéis poner el nombre del PC.

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i0.wp.com/1.bp.blogspot.com/_H4ctsPRjMs8/SjwBW5ajoNI/AAAAAAAAAJA/rEdNwr5kTnM/s1600-h/instant%C3%A1nea3.png" data-rel="lightbox-5" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 300px;" src="https://i2.wp.com/1.bp.blogspot.com/_H4ctsPRjMs8/SjwBW5ajoNI/AAAAAAAAAJA/rEdNwr5kTnM/s400/instant%C3%A1nea3.png" border="0" alt="" id="BLOGGER_PHOTO_ID_5349151950290264274" data-recalc-dims="1" /></a>

Siguiente, escogemos los drivers, y todo listo 😀