---
id: 217
title: 'Programando en C - Carrera'
date: 2011-01-26T23:05:40+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=217
permalink: /programacion/programando-en-c-carrera
aktt_notify_twitter:
  - no
syntaxhighlighter_encoded:
  - 1
views:
  - 279
categories:
  - Programación
tags:
  - bucle
  - c
  - corredores
  - cpp
  - funcion
  - registros
---
Este programa pide 5 corredores con sus respectivos tiempo, te muestra la clasificación y por último te devuelve el nombre del ganador y el tiempo medio.

<pre class="lang:c decode:1 " >#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_CORREDORES 5
#define MAX_CADENA 20

//Estructuras
struct Corredor{
char nombre[MAX_CADENA];
float tiempo;
};

//Prototipos de funciones
void leer_corredores(int numero_corredores, struct Corredor clasificacion[]);
void mostrar_corredores(int numero_corredores, struct Corredor clasificacion[]);
void calcular_datos(int numero_corredores, struct Corredor clasificacion[], float &media_tiempo, char ganador[]);

//Funcion main
int main()
{
float media_tiempo=0;
struct Corredor clasificacion[MAX_CORREDORES];
char ganador[MAX_CADENA];
leer_corredores(MAX_CORREDORES,clasificacion);
system("cls");
mostrar_corredores(MAX_CORREDORES,clasificacion);
getch();
system("cls");
calcular_datos(MAX_CORREDORES,clasificacion,media_tiempo,ganador);
printf("El ganador es %s",ganador);
printf("El tiempo medio ha sido %f",media_tiempo);
getch();
return 0;
}

//Funciones
void leer_corredores(int numero_corredores, struct Corredor clasificacion[])
{
int i;
for(i=0;i<numero_corredores;i++)
{
printf("\nNombre del corredor %d: ",i+1);
fflush(stdin);
gets(clasificacion[i].nombre);
printf("Tiempo: ");
scanf("%f", &clasificacion[i].tiempo);
}
}

void mostrar_corredores(int numero_corredores, struct Corredor clasificacion[])
{
int i;
for(i=0;i<numero_corredores;i++)
{
printf("El corredor %s ha hecho un tiempo de %.3f segundos" ,clasificacion[i].nombre, clasificacion[i].tiempo);
}
}

void calcular_datos(int numero_corredores, struct Corredor clasificacion[], float &media_tiempo, char ganador[])
{
int i;
float tiempo;
tiempo=clasificacion[0].tiempo;
ganador[0]='';
strcpy(ganador,clasificacion[0].nombre);
for(i=0;i<numero_corredores;i++)
{
media_tiempo+=clasificacion[i].tiempo;
if(clasificacion[i].tiempo<tiempo)
{
tiempo=clasificacion[i].tiempo;
ganador[0]='';
strcpy(ganador,clasificacion[i].nombre);
}
}
media_tiempo/=numero_corredores;
}
</pre>