---
id: 147
title: 'Programando en C - Primos menores que un numero x'
date: 2010-11-23T20:09:14+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=147
permalink: /programacion/primos-menores
syntaxhighlighter_encoded:
  - 1
views:
  - 89
categories:
  - Programación
tags:
  - c
  - cpp
  - primos
---
Este programa te muestra por pantalla todos los números primos menores al número que hayas introducido.  

He usado una función para saber si un número es primo, y un bucle para ir probando todos los números menores al número introducido.

```c
#include <stdio.h>
#include <conio.h>

void primo(unsigned num);

int main()
{
  unsigned veces,i;
  printf("Este programa va a motrar los numeros primos menores que n. Introduce n: ");
  scanf("%u",&veces);
  for(i=1;i<=veces;i++)
    primo(i);

  getch();
  return 0;
}

void primo(unsigned num)
{
  unsigned primo=1,divisor=2;
  if(num==1)
    printf("1t");
  else
  {    
    while((primo==1)&&(num!=divisor))
    {
      if(num%divisor==0)
        primo=0;
      else
        divisor++;
    }
    if(primo==1)
      printf("%ut",num);
  }
}
```
