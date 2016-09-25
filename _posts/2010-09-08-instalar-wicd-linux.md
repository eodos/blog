---
id: 5
title: Instalar wicd en Fedora 13 (o cualquier otra distribución)
date: 2010-09-08T01:02:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=5
permalink: /gnu-linux/instalar-wicd-linux
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2010/09/instalar-wicd-en-fedora-13-o-cualquier.html
blogger_author:
  - Eodos
views:
  - 117
categories:
  - GNU/Linux
  - Software
tags:
  - fedora
  - Kubuntu
  - Linux
  - wicd
---
<p style="text-align: center;">
  <a style="margin-left: 1em; margin-right: 1em;" href="http://upload.wikimedia.org/wikipedia/commons/thumb/7/78/Wicd_client_logo.png/120px-Wicd_client_logo.png" data-rel="lightbox-0" title=""><span style="color: #000000;"><img class="aligncenter" src="https://i2.wp.com/upload.wikimedia.org/wikipedia/commons/thumb/7/78/Wicd_client_logo.png/120px-Wicd_client_logo.png" border="0" alt="" data-recalc-dims="1" /></span></a>
</p>

**wicd** es un potente y sencillo gestor de redes, que te simplifica la tarea de gestionarlas, proceso que con KNetworkManager se puede volver muy engorroso. Además, funciona en cualquier plataforma y escritorio, al estar escrito en python.

En primer lugar, vamos a **preparar el entorno** añadiendo los paquetes ```python```, ```python-devel``` y ```wget```

**En Fedora 13:**

```bash
$ su -c 'yum -y install python python-devel wget'
```

**En Debian / Ubuntu / Kubuntu:**

```bash
$ sudo apt-get install python python-devel wget
```

Ahora vamos a bajar, **compilar e instalar wicd**. He usado la versión 1.7.0 del paquete, que cuando escribí el post era la más reciente. Puedes consultar en http://sourceforge.net/projects/wicd/files/ cual es la última versión y modificar lo que hay a continuación:

```bash
$ sudo cd /usr/local/src  
$ sudo wget http://downloads.sourceforge.net/wicd/wicd-1.7.0.tar.gz  
$ sudo tar -xf wicd-1.7.0.tar.gz  
$ sudo cd wicd-1.7.0  
$ sudo python setup.py configure  
$ sudo python setup.py install
```

Con ésto ya debemos tener wicd instalado. Ahora vamos a deshabilitar KNetworkManager (o el gestor que uses) y habilitar wicd:

```bash
$ sudo chkconfig NetworkManager off<br />
$ sudo chkconfig wicd on
```

Al reiniciar ya deberías tener wicd en funcionamiento 🙂

<p style="text-align: center;">
  <a style="margin-left: 1em; margin-right: 1em;" href="http://arbolcharyou.files.wordpress.com/2008/06/pantallazo-wicd-manager.png" data-rel="lightbox-1" title=""><span style="color: #000000;"><img class="aligncenter" src="https://i1.wp.com/arbolcharyou.files.wordpress.com/2008/06/pantallazo-wicd-manager.png?resize=385%2C400" border="0" alt="" data-recalc-dims="1" /></span></a>
</p>

<span style="font-family: Arial, Helvetica, sans-serif;">En Fedora he tenido algunos problemas con SELinux, por lo que he tenido que desactivarlo, se hace rápidamente editando el archivo <strong>/etc/selinux/config</strong></span><span style="font-family: Arial, Helvetica, sans-serif;"><br /> </span>

```bash
$ sudo kate /etc/selinux/config
```

<span style="font-family: Arial, Helvetica, sans-serif;">Y cambiamos <strong>SELINUX=enforcing</strong> por <strong>SELINUX=disabled</strong></span><span style="font-family: Arial, Helvetica, sans-serif;"><br /> </span>