---
id: 70
title: 'Python - Raices de un polinomio'
date: 2009-02-17T16:07:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=70
permalink: /desarrollo/python-raices-polinomio
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/02/python-programa-1-raices-de-un.html
blogger_author:
  - Eodos
views:
  - 58
categories:
  - Desarrollo
tags:
  - desarrollo
  - polinomio
  - Programación
  - Python
---
Ayer empecé con el Python, y hoy he compilado mi primer programa "útil", que calcula las soluciones (x) de un polinomio que escribimos.

Os pego el código por si queréis echarle un vistazo.

{% highlight python linenos=table %}
#!/usr/bin/env python
print "Este programa calculara las raices de un polinomio"
a=float(raw_input("Introduce el valor correspondiente a x^2 (a): "))
b=float(raw_input("Introduce el valor correspondiente a x (b): "))
c=float(raw_input("Introduce el valor correspondiente al termino independiente (c): "))
d=-float(b)
e=float(b)**2
f=4*float(a)float(c)
g=2*float(a)
h=float(e)-float(f)
i=float(h)**0.5
j=float(d)+float(i)
k=float(d)-float(i)
l=float(j)/float(g)
m=float(k)/float(g)
print "Las raices de " ,str(a) ,"x^2" ,str(b) ,"x" ,str(c) ,"son: " ,str(l) ,"y " ,str(m)
raw_input()
{% endhighlight %}
