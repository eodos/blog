---
id: 164
title: 'Programando en C - Los primeros x números primos'
date: 2010-11-23T23:51:00+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=164
permalink: /programacion/primeros-x-primos
syntaxhighlighter_encoded:
  - 1
views:
  - 80
categories:
  - Programación
tags:
  - c
  - cpp
  - primos
---
Este programa muestra los x primeros números primos, siendo x un valor introducido.

Lo he hecho con una función que determina si un número primo, y si lo es añade 1 al contador, mediante una función de paso de parámetro.

<pre class="lang:c decode:1 " >#include <stdio.h>
#include <conio.h>

void primo(unsigned num, unsigned &contador);

int main()
{
    unsigned veces,i=1,contador=1;
    printf("Este programa va a motrar los n primeros primos. Introduce n: ");
    scanf("%u",&veces);
    while(contador<=veces)
    {
        primo(i,contador);
        i++;
    }
    getch();
    return 0;
}

void primo(unsigned num, unsigned &contador)
{
    unsigned primo=1,divisor=2;
    if(num==1)
    {
        printf("1t");
        contador++;
    }
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
        {
            contador++;
            printf("%ut",num);
        }
    }
}

</pre>
