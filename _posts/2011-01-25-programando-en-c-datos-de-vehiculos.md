---
id: 213
title: 'Programando en C - Datos de vehículos'
date: 2011-01-25T21:55:07+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=213
permalink: /programacion/programando-en-c-datos-de-vehiculos
aktt_notify_twitter:
  - no
syntaxhighlighter_encoded:
  - 1
views:
  - 280
categories:
  - Programación
tags:
  - arrays
  - c
  - cpp
  - registros
  - vectores
  - vehiculos
---
Se corresponde con el ejercicio 4 de la relación.

<pre class="lang:c decode:1 " >//Ejercicio 4

#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_VEHICULOS 30

struct vehiculo{
       char marca;
       char modelo[21];
       unsigned long kilometros;
       char matricula[8];
};

int leer_vehiculos(int num_vehiculos, struct vehiculo lista_vehiculos[], int maximo, int &vehiculos_hay);

int main()
{
    struct vehiculo lista_vehiculos[MAX_VEHICULOS];
    int vehiculos_hay=0, exito;
    
    exito=leer_vehiculos(2,lista_vehiculos,MAX_VEHICULOS,vehiculos_hay);
    
    
    getch();
    return 0;
}

int leer_vehiculos(int num_vehiculos, struct vehiculo lista_vehiculos[], int maximo, int &vehiculos_hay)
{
    int exito=0, i, j=1;
    if(vehiculos_hay+num_vehiculos<=maximo)
    {
        exito=1;
        vehiculos_hay+=num_vehiculos;
        for(i=vehiculos_hay;i<vehiculos_hay+num_vehiculos;i++)
        {
            printf("\nIntroduce los datos del vehiculo %d",j);
            printf("\nIncial de la marca: ");
            fflush(stdin);
            scanf("%c",&lista_vehiculos[i].marca);
            printf("Modelo del vehiculo (max 20 caracteres): ");
            fflush(stdin);
            gets(lista_vehiculos[i].modelo);
            printf("Kilometros: ");
            scanf("%lu",&lista_vehiculos[i].kilometros);
            printf("Matricula: ");
            fflush(stdin);
            gets(lista_vehiculos[i].matricula);
            j++;
            system("cls");            
        }
        printf("Todos los vehiculos han sido introducidos correctamente");
        getch();
    }
    else
    {
        printf("Ha sobrepasado el limite de vehiculos, unicamente puede añadir %d",maximo-vehiculos_hay);
        getch();
    }
    return exito;
}
</pre>
