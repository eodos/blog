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
Hace unos días, recibí mi portátil Vaio serie F (VPCF126FM). Es una pasada: lleva un i7, 6 GB de memoria RAM, una GeForce 330&#8230;.

De serie incluye Windows 7 Home Edition. Y como defensor del software libre que soy, pues me propuse instalarle una distribución GNU/Linux 🙂

La elegida en esta ocasión fue Debian en su versión estable (5.05). El motivo fue que estoy&nbsp;acostumbrado&nbsp;a usar Kubuntu, y me apetecía probar su distribución &#8220;madre&#8221;. Durante la instalación surgieron estos dos problemas, que en principio ignoré para intentar arreglarlos más adelante:

  * El touchpad no funciona.
  * No reconoce la tarjeta de red.

<div>
  Sobre el touchpad, se solucionó fácilmente añadiendo el parámetro &#8220;<b>i8042.nopnp</b>&#8221; al boot del kernel. Para hacerlo, al inciar el ordenador en el grub os colocáis sobre vuestra distribución Debian y pulsáis &#8220;e&#8221;. Vais a la línea del kernel y&nbsp;volvéis&nbsp;a pulsar &#8220;e&#8221;. Os situáis al final de la línea, y escribís ese parámetro sin comillas. Agradezco a ESC201 en su <a href="http://code.google.com/p/vaio-f11-linux/issues/detail?id=1#c21">entrada</a>, que me ayudó a resolver el problema.
</div>

<div>
</div>

<div>
  Sobre la tarjeta de red, la cosa se complica. Este Vaio lleva una tarjeta Atheros AR9287, que sólo es soportada por el driver <b>ath9k</b>. Este driver sólo es compatible con un kernel superior al 2.6.27-rc3, y, pobre de mí, mi Debian llevaba la 2.6.26-2 🙁
</div>

<div>
</div>

<div>
  Al final me he propuesto instalarle Fedora 13, una distribución muy actualizada que incorpora el kernel 2.6.33, que soporta sin problemas la tarjeta.
</div>

<div>
</div>

<div>
  Ya os contaré en la siguiente entrega 🙂
</div>

<div>
</div>

<div>
</div>