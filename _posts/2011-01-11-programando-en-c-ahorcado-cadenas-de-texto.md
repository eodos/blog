---
id: 205
title: 'Programando en C - Ahorcado (cadenas de texto)'
date: 2011-01-11T23:36:25+00:00
author: eodos
layout: post
guid: http://eodos.es/?p=205
permalink: /programacion/programando-en-c-ahorcado-cadenas-de-texto
syntaxhighlighter_encoded:
  - 1
aktt_notify_twitter:
  - yes
aktt_tweeted:
  - 1
views:
  - 307
categories:
  - Programación
tags:
  - ahorcado
  - arrays
  - c
  - cadenas de texto
  - gets
  - puts
---
<pre class="lang:c decode:1 " >#include <stdio.h>
#include <conio.h>
#include <string.h>
#include <stdlib.h>

void cifrar_cadena(char *cadena,char *cadena_cifrada);
int es_letra(char letra);

int main()
{
    char cadena[100],cadena_cifrada[100],letra;
    int intentos=10,i,j,acierto,fin=0;
    gets(cadena);
    system("cls");
    cifrar_cadena(cadena,cadena_cifrada);
    puts(cadena_cifrada);
    do
    {
        printf("\nIntroduce una letra: ");
        fflush(stdin);
        scanf("%c",&letra);
        fflush(stdin);
        acierto=0;
        for(i=0;i<strlen(cadena);i++)
        {
            if((cadena[i]==letra||cadena[i]==(letra-32))&&cadena_cifrada[i]!=letra)
            {
                cadena_cifrada[i]=letra;
                puts(cadena_cifrada);
                acierto=1;
            }
        }
        if(strcmp(cadena,cadena_cifrada)==0)
            fin=1;
        if(acierto==0)
        {
            if(intentos>0)
            {
                printf("Has fallado, te quedan %d intentos",intentos);
                intentos--;
            }          
        }  
    }
    while(intentos!=0&&fin==0);
    if(fin==1)
        printf("\nEnhorabuena, has ganado!!!!");
    else
        printf("\n\nLo siento, has perdido");        
    getch();
    return 0;
}
    
void cifrar_cadena(char *cadena,char *cadena_cifrada)
{
     int tamano,i;
     tamano=strlen(cadena);
     strcpy(cadena_cifrada,cadena);
     for(i=1;i<tamano-1;i++)
         if(es_letra(cadena_cifrada[i]))
             if(cadena[i]!=cadena[0]&&cadena[i]!=cadena[tamano-1])
                 cadena_cifrada[i]='*';
}

int es_letra(char letra)
{
    return((letra>='a'&&letra<='z')||(letra>='A'&&letra<='Z'));
}
</pre>
