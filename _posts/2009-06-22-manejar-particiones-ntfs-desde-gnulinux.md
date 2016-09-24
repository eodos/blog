---
id: 34
title: Manejar particiones NTFS desde GNU/Linux
date: 2009-06-22T22:14:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=34
permalink: /gnu-linux/manejar-particiones-ntfs-desde-gnulinux
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/06/manejar-particiones-ntfs-desde-gnulinux.html
blogger_author:
  - Eodos
views:
  - 43
categories:
  - GNU/Linux
tags:
  - Linux
---
Seguramente muchos tendréis alguna partición en NTFS, ya sea con Windows o usado como almacén.  
Las últimas distribuciones de Linux ya traen el soporte a NTFS integrado, pero por si usáis alguna más antigua, os indico los pasos para poder escribir y leer en ellas.

Instalar los paquetes ntfs-config y ntfs-3g:

    <span style="font-weight:bold;">sudo apt-get install ntfs-3g</p> 

<p>
  sudo apt-get install ntfs-config </span>
</p>

<p>
  Ejecutamos <span style="font-weight:bold;">ntfs-config</span> para dar los permisos de unidades externas e internas.
</p>

<p>
  Para que las particiones en NTFS se monten automáticamente al inicio de nuestro SO, debemos modificar el archivo fstab (si usais Gnome, sustituid kate por gedit).
</p>

<p>
  <span style="font-weight:bold;"> sudo kate /etc/fstab</span>
</p>

<p>
  Añadimos o modificamos la entrada de la partición:
</p>

<p>
  <span style="font-weight:bold;">/dev/sda1 /media/Almacen ntfs-3g rw,users,gid=users,umask=0002 0 0</span>
</p>