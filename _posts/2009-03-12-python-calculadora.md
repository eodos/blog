---
id: 66
title: 'Python - Calculadora'
date: 2009-03-12T00:46:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=66
permalink: /desarrollo/python-calculadora
blogger_blog:
  - eodos0.blogspot.com
blogger_permalink:
  - /2009/03/python-programa-5-calculadora.html
blogger_author:
  - Eodos
views:
  - 180
categories:
  - Desarrollo
tags:
  - Python
---
<pre class="lang:python decode:true ">#!/usr/bin/env python

#Datos del programa:
#Programa: Calculadora.
#Funcion: Realiza diversas operaciones entre dos numeros.
#Autor: Eodos.
#Fecha: 21 Febrero 2009.

#Introduccion
print "Calculadora"
operacion= ""

#Condicional de salida
while operacion!=7:

#Entrada de datos
  numero1=float(raw_input("Escribe un numero: "))
  numero2=float(raw_input("Escribe otro numero: "))

#Operaciones
  print "1. Suma"
  print "2. Resta"
  print "3. Division"
  print "4. Multiplicacion"
  print "5. Potencia"
  print "6. Raiz cuadrada"
  print "7. Salir"

#Entrada de operacion
  operacion=float(raw_input("Operacion:(1,2,3...) "))

#Suma
  if operacion==1:
    suma=float(numero1)+float(numero2)
    print str(suma)

#Resta
  elif operacion==2:
    print "1." ,str(numero1) ,"-" ,str(numero2)
    print "2." ,str(numero2) ,"-" ,str(numero1)
    tiporesta=int(raw_input("1 o 2: "))
    if tiporesta==1:
      resta=float(numero1)-float(numero2)
      print str(resta)
    elif tiporesta==2:
      resta=float(numero2)-float(numero1)
      print str(resta)

#Division
  elif operacion==3:
    print "1." ,str(numero1) ,"entre" ,str(numero2)
    print "2." ,str(numero2) ,"entre" ,str(numero1)
    tipodivision=int(raw_input("1 o 2: "))
    if tipodivision==1:
      if numero2==0:
        print "No puedes dividir entre 0"
      else:
        division=float(numero1)/float(numero2)
        print str(division)
    elif tipodivision==2:
     if numero1==0:
       print "No puedes dividir entre 0"
     else:
       division=float(numero2)/float(numero1)
       print str(division)

#Multiplicacion
  elif operacion==4:
    multiplicacion=float(numero1)*float(numero2)
    print str(multiplicacion)

#Potencia
  elif operacion==5:
    print "1." ,str(numero1) ,"elevado a" ,str(numero2)
    print "2." ,str(numero2) ,"elevado a" ,str(numero1)
    tipopotencia=int(raw_input("1 o 2: "))
    if tipopotencia==1:
      potencia=float(numero1)**float(numero2)
      print str(potencia)
    elif tipopotencia==2:
      potencia=float(numero2)**float(numero1)
      print str(potencia)

#Raiz cuadrada
  elif operacion==6:
    print "1. Raiz cuadrada de" ,str(numero1)
    print "2. Raiz cuadrada de" ,str(numero2)
    print "3. Raiz cuadrada de" ,str(numero1) ,"mas raiz cuadrada de" ,str(numero2)
    print "4. Raiz cuadrada de" ,str(numero1) ,"+" ,str(numero2)
    tiporaiz=int(raw_input("1, 2, 3 o 4: "))
    if tiporaiz==1:
      if numero1<0:
        print "La raiz no tiene soluciones reales"
      else:
        raiz=float(numero1)**0.5
        print str(raiz)
    elif tiporaiz==2:
        if numero2<0:
          print "La raiz no tiene soluciones reales"
        else:  
          raiz=float(numero2)**0.5
          print str(raiz)
    elif tiporaiz==3:
       if numero1         print "La raiz no tiene soluciones reales"
       else:
         raiz1=float(numero1)**0.5
         raiz2=float(numero2)**0.5
         raiz=float(raiz1)+float(raiz2)
         print str(raiz)
    elif tiporaiz==4:
      sumaraiz=float(numero1)+float(numero2)
      if sumaraiz<0:
        print "La raiz no tiene soluciones reales"
      else:
        raiz=float(sumaraiz)**0.5
        print str(raiz)

if operacion==7:
  print "Gracias por usar el programa"
  raw_input()</pre>

&nbsp;
