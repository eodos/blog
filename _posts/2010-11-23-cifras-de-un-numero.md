---
id: 167
title: 'Programando en C - Cifras de un número'
date: 2010-11-23T23:53:09+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=167
permalink: /programacion/cifras-de-un-numero
syntaxhighlighter_encoded:
  - 1
views:
  - 136
categories:
  - Programación
tags:
  - c
  - cifras
  - cpp
---
Este programa va a calcular el número de cifras de un número de una forma muy sencilla, que posteriormente se puede usar como función para extraer dígitos.

```c
#include <stdio.h>
#include <conio.h>

int main()
{
    int num,contador=1;
    printf("Este programa te va a decir cuantos digitos tiene un numero. Introduce uno: ");
    scanf("%d",&num);

    while(num/10>0)
    {
        num=num/10;
        contador++;
    }
    printf("Tiene %u caracteres",contador);
    getch();
    return 0;
}
```
