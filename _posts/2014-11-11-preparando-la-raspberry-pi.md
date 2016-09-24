---
id: 432
title: 'PFG - Preparando la Raspberry PI'
date: 2014-11-11T18:36:54+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=432
permalink: /proyectos/preparando-la-raspberry-pi
categories:
  - Proyectos
tags:
  - archlinux
  - archlinux arm
  - arm
  - raspberry
  - raspberry pi
  - raspberrypi
  - wifi
---
<a href="https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/Raspberry-Pi2.jpg" data-rel="lightbox-0" title=""><img class="alignleft wp-image-433 size-medium" src="https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/Raspberry-Pi2-300x211.jpg?resize=300%2C211" alt="Raspberry Pi" srcset="https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/Raspberry-Pi2.jpg?resize=300%2C211&ssl=1 300w, https://i0.wp.com/eodos.net/wp-content/uploads/2014/11/Raspberry-Pi2.jpg?w=650&ssl=1 650w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>

<p style="text-align: justify;">
  Se ha escogido como Sistema Operativo Archlinux ARM v6 al ser un sistema sin entorno gráfico ni aplicaciones preinstaladas. Para instalarlo en la SD he seguido los siguientes pasos:
</p>

<ol style="text-align: justify;">
  <li>
    Particionar la SD con <em>fdisk</em>, creando una partición <em>boot</em> de 100 MB y dedicando el resto del espacio disponible a la partición raiz.
  </li>
  <li>
    Descomprimir la última versión de Archlinux ARM v6 para Raspberry PI y copiarla en las particiones creadas.
  </li>
</ol>

<p style="text-align: justify;">
  He usado una antena Wi-Fi USB para la conexión de la Raspberry a internet. Para configurarla, primero conectamos la Raspberry por HDMI a un monitor y nos conectamos por ethernet para poder descargar unas dependencias necesarias:
</p>

<pre class="lang:sh decode:true">> ip link set eth0 up
> ip addr add 192.168.2.200/24 broadcast 192.168.2.255 dev eth0
> ip route add default via 192.168.2.1</pre>

<p style="text-align: justify;">
  A continuación añadimos las direcciones DNS a usar en el fichero <em>/etc/resolv.conf</em> e instalamos los paquetes necesarios para la conexión WiFi:
</p>

<pre class="lang:sh decode:true">> pacman -S iw dialog wpa-supplicant</pre>

<p style="text-align: justify;">
  Ahora creamos un perfil de netctl a través de wifi-menu y lo activamos para que se inicie automáticamente al iniciar la Raspberry:
</p>

<pre class="lang:sh decode:true">> netctl start wlan0-dd-wrt</pre>

<p style="text-align: justify;">
  Y ya podemos desconectar el cable ethernet, el cable HDMI y conectarnos con ssh a la Raspberry Pi.
</p>

 Como configuraciones adicionales cambiado el **hostname** a **raspberry **en los ficheros _/etc/hostname_, _/etc/hosts_ y he ejecutado la instrucción

<pre class="lang:sh decode:true ">> hostname raspberry</pre>

&nbsp;