---
id: 6
title: Linux en Sony Vaio serie F (II Parte)
date: 2010-09-08T00:32:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=6
permalink: /gnu-linux/linux-vaio-f-ii
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2010/09/linux-en-sony-vaio-serie-f-ii-parte.html
blogger_author:
  - Eodos
views:
  - 82
categories:
  - GNU/Linux
tags:
  - Debian
  - fedora
  - Linux
  - Vaio
---
Finalmente, decidí instalar Fedora 13 con el escritorio KDE en su versión x64.


Los problemas que sufrí en Debian, se solucionaron instantáneamente 🙂


Para el touchpad, fue suficiente con añadir *"i8042.nopnp"* a la entrada del kernel en el grub. La tarjeta de red también funcionó directamente. Únicamente tuve problemas para autentificarme en redes WEP, que lo solucioné cambiando la contraseña por una WPA. Intentaré solucionarlo más adelante.


También probé con otros gestores de red, ya que KNetworkManager me parece poco potente e ineficaz. **wicd** funcionó a la perfección, aunque tampoco me andaban las redes WEP.


Tanto el sonido como la aceleración 3D, funcionaron perfectamente y al instante, sin que tuviera que hacer nada.


Por lo tanto, misión cumplida 🙂