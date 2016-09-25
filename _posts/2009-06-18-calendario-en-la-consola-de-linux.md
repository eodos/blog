---
id: 38
title: Calendario en la Consola de Linux
date: 2009-06-18T23:52:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=38
permalink: /gnu-linux/calendario-en-la-consola-de-linux
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/06/calendario-en-la-consola-de-linux.html
blogger_author:
  - Eodos
views:
  - 14
categories:
  - GNU/Linux
tags:
  - Linux
---
<a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="https://i1.wp.com/microteknologias.files.wordpress.com/2009/04/calendario-linux.jpg"><img style="display:block; margin:0px auto 10px; text-align:center;cursor:pointer; cursor:hand;width: 288px; height: 299px;" src="https://i1.wp.com/microteknologias.files.wordpress.com/2009/04/calendario-linux.jpg" border="0" alt="" data-recalc-dims="1" /></a>

Si deseas consultar una determinada fecha y no tienes un calendario a la mano, puedes utilizar el útil comando:
  
```bash
$ cal
```

En una consola de Linux y obtendrás un sencillo calendario.

Si escribes únicamente cal, el sistema mostrará el mes actual con el domingo como primer día de la semana. Si deseas que el lunes aparezca como primer día, debes escribir:

```bash
$ calc -m
```

Ahora, si deseas visualizar un calendario con todos los meses, solo debes añadir el año que deseas. Por ejemplo para visualizar el calendario del 2010 con el lunes como primer día, escribe el siguiente comando:

```bash
$ cal -m 2010
```

También puedes visualizar 3 meses, el anterior, el actual y el próximo, con el argumento -3. Por ejemplo, para visualizar marzo, abril y mayo del presente año, escribimos el siguiente comando:
  
```bash
$ cal -3m 4 </p>
```

El 4 (abril) indica el mes que estará en el centro.

Otro comando que ofrece una prestaciones similares y más, es gcal. Como siempre, puedes obtener mucha información consultando man gcal o info gcal.

Fuente | [TuxApuntes](http://www.tuxapuntes.com/drupal/node/1283)
