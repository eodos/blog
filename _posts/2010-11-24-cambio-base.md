---
id: 171
title: 'Programando en C - Cambio de base'
date: 2010-11-24T00:24:11+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=171
permalink: /programacion/cambio-base
syntaxhighlighter_encoded:
  - 1
views:
  - 318
categories:
  - Programación
tags:
  - c
  - cambio de base
  - cpp
---
Este sencillo algoritmo permite cambiar de un número natural en base 10 a cualquier base que quieras.

```c
#include <stdio.h>
#include <conio.h>

int main()
{
  unsigned num,base,total=0,coeficiente=1;
  printf("Este programa va a pasar un numero natural en base 10 a una determinada base. Introduce el numero y la base: ");
  scanf("%u %u",&num,&base);
  while(num!=0)
  {
    total=total+coeficiente*(num%base);
    num=num/base;
    coeficiente*=10;
  }
  printf("%u",total);
  getch();
  return 0;
}
```
