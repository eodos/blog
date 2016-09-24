---
id: 37
title: 'Compartir impresora. Linux -> WinXP'
date: 2009-06-19T14:44:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=37
permalink: /gnu-linux/compartir-impresora-linux-winxp
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/06/compartir-impresora-linux-winxp.html
blogger_author:
  - Eodos
views:
  - 65
categories:
  - GNU/Linux
  - Windows
tags:
  - Linux
  - Windows
---
EL otro día cansado de tener que imprimir todos los documentos que hace mi hermano y mi madre, me puse a configurar la impresora por Lan, para que mi hermano pudiera usarla desde su ordenador con Windows XP SP2.

En <span style="font-weight:bold;">Linux</span>

Lo primero que tenéis que hacer, es abrir el cups (el &#8220;driver&#8221; de impresión en Unix). Podéis acceder desde:

- Vuestra ip interna con el puerto 631    
192.168.1.*:631

- Desde localhost:    
127.0.0.1:631

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i2.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjuLPOQuYEI/AAAAAAAAAII/zZ_hYB2PVcA/s1600-h/cups.png" data-rel="lightbox-0" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 320px;" src="https://i2.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjuLPOQuYEI/AAAAAAAAAII/zZ_hYB2PVcA/s400/cups.png" border="0" alt="" id="BLOGGER_PHOTO_ID_5349022076075139138" data-recalc-dims="1" /></a>

Click en Impresoras

<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i0.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjuLe3bf54I/AAAAAAAAAIQ/jia8sszAUi8/s1600-h/dx.png" data-rel="lightbox-1" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 400px; height: 320px;" src="https://i2.wp.com/4.bp.blogspot.com/_H4ctsPRjMs8/SjuLe3bf54I/AAAAAAAAAIQ/jia8sszAUi8/s400/dx.png" border="0" alt="" id="BLOGGER_PHOTO_ID_5349022344824219522" data-recalc-dims="1" /></a>

Ahí podemos observar el puerto al que está conectado la impresora, y el nombre.

Ahora vamos a <span style="font-weight:bold;">Windows</span>

Lo primero de todo, es instalar los drivers, como la mía es epson, los bajé de la web de Epson.  
Panel de control, impresoras, agregar nueva impresora.  
Escogemos la opción de impresora por Lan.

En dirección, escribimos:

http://ip:631/printers/nombre_impresora

Por ejemplo:  
<span style="font-weight:bold;"><br />http://192.168.1.36:631/printers/StylusDX6000</span>

Y escogemos el driver que hemos instalado antes.

Fin XD

Nótese que el ordenador anfitrion debe estar encendido para poder imprimir desde el otro.