---
id: 7
title: Linux en Sony Vaio serie F (I Parte)
date: 2010-08-17T00:37:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=7
permalink: /gnu-linux/linux-vaio-f-i
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2010/08/problemas-en-sony-vaio-f12-al-instalar.html
blogger_author:
  - Eodos
views:
  - 109
categories:
  - GNU/Linux
tags:
  - Debian
  - Linux
  - Vaio
---
Hace unos días, recibí mi portátil **Vaio serie F (VPCF126FM)**. Es una pasada: lleva un i7, 6 GB de memoria RAM, una GeForce 330....

De serie incluye Windows 7 Home Edition. Y como defensor del software libre que soy, pues me propuse instalarle una distribución GNU/Linux 🙂

La elegida en esta ocasión fue Debian en su versión estable (5.05). El motivo fue que estoy acostumbrado a usar Kubuntu, y me apetecía probar su distribución "madre". Durante la instalación surgieron estos dos problemas, que en principio ignoré para intentar arreglarlos más adelante:

* El touchpad no funciona.
* No reconoce la tarjeta de red.


Sobre el touchpad, se solucionó fácilmente añadiendo el parámetro "<b>i8042.nopnp</b>" al boot del kernel. Para hacerlo, al inciar el ordenador en el grub os colocáis sobre vuestra distribución Debian y pulsáis "e". Vais a la línea del kernel y volvéis a pulsar "e". Os situáis al final de la línea, y escribís ese parámetro sin comillas. Agradezco a ESC201 en su <a href="http://code.google.com/p/vaio-f11-linux/issues/detail?id=1#c21">entrada</a>, que me ayudó a resolver el problema.


Sobre la tarjeta de red, la cosa se complica. Este Vaio lleva una tarjeta Atheros AR9287, que sólo es soportada por el driver <b>ath9k</b>. Este driver sólo es compatible con un kernel superior al 2.6.27-rc3, y, pobre de mí, mi Debian llevaba la 2.6.26-2 🙁


Al final me he propuesto instalarle Fedora 13, una distribución muy actualizada que incorpora el kernel 2.6.33, que soporta sin problemas la tarjeta.


Ya os contaré en la siguiente entrega 🙂