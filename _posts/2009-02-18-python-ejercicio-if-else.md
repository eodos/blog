---
id: 68
title: 'Python - Tu edad (Ejercicio If, Else)'
date: 2009-02-18T21:09:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=68
permalink: /desarrollo/python-ejercicio-if-else
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/02/python-programa-3-tu-edad-ejercicio-if.html
blogger_author:
  - Eodos
views:
  - 34
categories:
  - Desarrollo
tags:
  - condicional
  - condicionales
  - desarrollo
  - else
  - if
  - Python
---
Tras leerme la lección de condicionales he decidido compilar algún programa sencillo.

<pre class="lang:python decode:true ">#!/usr/bin/env python
edad = int(raw_input('Cuantos anos tienes '))
if edad>=90:
  print "Tienes 90 anos o mas..."
else:
  if edad<=0:  
    print "Has metido una edad no valida"
  else:  
    if edad<18:
       print 'Eres menor de edad'
    else:
       print 'Eres mayor de edad'
print 'Hasta la proxima'
raw_input()</pre>

&nbsp;
