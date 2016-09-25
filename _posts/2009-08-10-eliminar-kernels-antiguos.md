---
id: 26
title: Eliminar kernels antiguos
date: 2009-08-10T15:02:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=26
permalink: /gnu-linux/eliminar-kernels-antiguos
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/08/eliminar-kernels-antiguos.html
blogger_author:
  - Eodos
views:
  - 82
categories:
  - GNU/Linux
tags:
  - Kernel
  - Kubuntu
  - Linux
  - Ubuntu
---
Si recientemente habéis actualizado vuestra distribución, y habéis instalado un kernel más reciente, en el menú del grub os aparecerán todos los kernels instalados. Para eliminar los antiguos sólo tenéis que abrir una terminal y:

```bash
$ dpkg -get-selections | grep linux-image
```

Este comando os creará un listado de los kernels instalados, en mi caso tengo:

```
linux-image-2.6.28-11-generic install  
linux-image-2.6.28-14-generic install  
linux-image-generic install
```

Así que voy a desinstalar linux-image-2.6.28-11-generic:

```bash
$ sudo aptitude purge linux-image-2.6.28-11-generic
```

A continuación vamos a actualizar el menu.list:

```bash
$ kdesudo kate /boot/grub/menu.list
```

O si usáis gnome:

```bash
$ sudo gedit /boot/grub/menu.list
```

A continuación buscamos las entradas del kernel eliminado y las borramos, en mi caso:

```
title Kubuntu 9.04, kernel 2.6.28-11-generic  
uuid c27d33e0-f7e1-41bb-99f2-7cb6e4316709  
kernel /vmlinuz-2.6.28-11-generic root=UUID=712fe989-f836-4b24-b771-b6a26a44096b ro quiet splash   
initrd /initrd.img-2.6.28-11-generic  
quiet

title Kubuntu 9.04, kernel 2.6.28-11-generic (recovery mode)  
uuid c27d33e0-f7e1-41bb-99f2-7cb6e4316709  
kernel /vmlinuz-2.6.28-11-generic root=UUID=712fe989-f836-4b24-b771-b6a26a44096b ro single  
initrd /initrd.img-2.6.28-11-generic
```