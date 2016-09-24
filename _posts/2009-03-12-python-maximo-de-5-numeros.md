---
id: 67
title: 'Python - Máximo de 5 números'
date: 2009-03-12T00:45:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=67
permalink: /desarrollo/python-maximo-de-5-numeros
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/03/python-programa-4-maximo-de-5-numeros.html
blogger_author:
  - Eodos
views:
  - 87
categories:
  - Desarrollo
tags:
  - desarrollo
  - maximo
  - numeros
  - Python
---
Este programa lo hice hace tiempo, pero lo pongo como recopilación.

Uno que compara 5 números y te dice el mayor:

<pre class="lang:python decode:true ">#!/usr/bin/env python

print "Este programa te permite calcular el numero mas alto de 5 numeros."

a=float(raw_input("Escribe un numero: "))
b=float(raw_input("Escribe otro numero: "))
c=float(raw_input("Escribe otro numero: "))
d=float(raw_input("Escribe otro numero: "))
e=float(raw_input("Escribe otro numero: "))

if a==b:
  maximo=a
if a>b:
  maximo=a
if b>a:
  maximo=b
if c>maximo:
  maximo=c
if d>maximo:
  maximo=d
if e>maximo:
  maximo=e

print "El numero mas alto es:" ,str(maximo)</pre>

&nbsp;
