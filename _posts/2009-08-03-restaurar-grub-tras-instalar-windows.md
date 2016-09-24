---
id: 29
title: Restaurar Grub tras instalar Windows
date: 2009-08-03T15:16:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=29
permalink: /gnu-linux/restaurar-grub-tras-instalar-windows
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/08/restaurar-grub-tras-instalar-windows.html
blogger_author:
  - Eodos
views:
  - 84
categories:
  - GNU/Linux
tags:
  - Grub
  - Linux
  - Windows
---
<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i2.wp.com/mepicanlosdedos.files.wordpress.com/2008/11/grub1.jpg" data-rel="lightbox-0" title=""><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 480px; height: 305px;" src="https://i2.wp.com/mepicanlosdedos.files.wordpress.com/2008/11/grub1.jpg" border="0" alt="" data-recalc-dims="1" /></a>

Cuándo tenéis GNU/Linux e instaláis Windows en otra partición, seguramente vuestro Grub será sustituído por el MBR de Windows, y no podréis entrar a vuestra distro.

Para solucionarlo, y volver a poner el Grub, tenéis que:

1. Bootear el live CD de alguna distro GNU/Linux (por ejemplo Kubuntu 9.04).  
2. Abrir una terminal y escribid:

> sudo grub

> find /boot/grub/stage1

La salida será estilo &#8220;(hd0,2)&#8221;. Siendo 0 y 2 dos número que varían según tus particiones.

> root (hd0,2)

> setup (hd0)

> quit

Con esto, ya tendréis el Grub cuando reinicieis el ordenador. Ahora vamos a ver cómo añadir el Windows al grub, y así poder elegir cuando se encienda el ordenador.

Ya en nuestra distro:

1. Vamos a la consola y escribimos:

> sudo kate /boot/grub/menu.lst

Sustituid por &#8220;gedit&#8221; si usáis gnome, o por vuestro editor favorito.

2. Añadimos las siguientes líneas:

> \# This entry is for Windows 7  
> title Windows 7  
> root (hd0,0)  
> chainloader +1

Tenéis que sustituir (hd0,0) por la partición en la que tengáis Windows.  
Lo podéis ver desde gparted, o cualquier editor de particiones.

Por ejemplo si lo tenéis en /dev/sda3, pues restáis 1 y os queda (hd0,2)

Un saludo 🙂