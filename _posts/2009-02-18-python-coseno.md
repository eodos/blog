---
id: 69
title: 'Python - Coseno'
date: 2009-02-18T17:42:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=69
permalink: /desarrollo/python-coseno
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/02/python-programa-2-coseno.html
blogger_author:
  - Eodos
views:
  - 36
categories:
  - Desarrollo
tags:
  - coseno
  - desarrollo
  - liberías
  - Python
---
Bueno hoy he hecho mi primer programa importando modulos. Este en concreto calcula el valor del coseno de un angulo dado en radianes. Se puede modificar para aceptarlos en grados decimales&#8230;

<pre class="lang:python decode:true ">#!/usr/bin/env python
from math import cos
a=float(raw_input("Introduce el valor en radianes: "))
b=cos(a)
print "El coseno es " ,str(b)
raw_input()</pre>

&nbsp;
