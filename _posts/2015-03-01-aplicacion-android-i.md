---
id: 501
title: 'PFG - Aplicación Android I'
date: 2015-03-01T12:25:17+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=501
permalink: /proyectos/aplicacion-android-i
categories:
  - Proyectos
tags:
  - android
  - asynctask
  - get
  - http
  - pfg
  - post
---
<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2015/03/riego.png" data-rel="lightbox-0" title=""><img class="  wp-image-512 alignright" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/03/riego.png?resize=286%2C491" alt="riego" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/03/riego.png?w=584&ssl=1 584w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/03/riego.png?resize=175%2C300&ssl=1 175w" sizes="(max-width: 286px) 100vw, 286px" data-recalc-dims="1" /></a>El objetivo de esta etapa es crear una aplicación sencilla para Android que sea capaz de establecer conexión con la Raspberry Pi (estando conectadas a la misma red local) y ejecutar un script CGI alojado en ella.

Como primera aproximación al problema lo que se realizará una petición GET sobre la URL donde se ubica el script y se mostrará la salida en un widget ****TextView****.

La aplicación se ha desarrollado para dispositivos que usen al menos la versión 4.0.3 de Android usando el SDK API 15. Se estima que un 90,4% de los dispositivos móviles cumplen este requisito.

Tras crear la aplicación, se ha definido una _clase privada_ **DownloadWebPageTask** en la actividad principal (MainActivity). En primera aproximación se realizó directamente la ejecución de la URL destino desde esta clase, dando como resultado un error ya que el thread principal de la aplicación no puede realizar peticiones web.

Se decide por tanto hacer uso de la clase **AsyncTask** para realizar esta operación en segundo plano sin tener que procuparse de la gestión de los threads.

La petición web sobre el script CGI se realiza usando instrucciones de la librería HTTP de Android (org.apache.http). Para mostrar la salida en el widget TextView ha sido necesario hacer uso del scope _fromHtml()_ para que se ejecuten las etiquetas HTML de la salida.

Adicionalmente ha sido necesario dar permisos a la aplicación Android para poder conectarse a internet, incluyendo la siguiente línea en el fichero **AndroidManifest.xml**

<pre class="lang:xhtml decode:true"><uses-permission android:name="android.permission.INTERNET" /></pre>

## Codigo

A continuación adjunto el código de la aplicación

### MainActivity.java

<pre class="lang:java decode:true">package net.eodos.controlderiego;

import android.os.AsyncTask;
import android.os.Bundle;
import android.support.v7.app.ActionBarActivity;
import android.text.Html;
//import android.view.Menu;
//import android.view.MenuItem;
import android.view.View;
import android.widget.TextView;

import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;

public class MainActivity extends ActionBarActivity {
    private TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Referencia a controles de la interfaz
        textView = (TextView) findViewById(R.id.TextView01);
    }

    private class DownloadWebPageTask extends AsyncTask<String, Void, String> {
        @Override
        protected String doInBackground(String... urls) {
            String response = "";
            for (String url : urls) {
                DefaultHttpClient client = new DefaultHttpClient();
                HttpGet httpGet = new HttpGet(url);
                try {
                    HttpResponse execute = client.execute(httpGet);
                    InputStream content = execute.getEntity().getContent();

                    BufferedReader buffer = new BufferedReader(new InputStreamReader(content));
                    String s = "";
                    while ((s = buffer.readLine()) != null) {
                        response += s;
                    }

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return response;
        }

        @Override
        protected void onPostExecute(String result) {
            textView.setText(Html.fromHtml(result));
        }
    }

    public void onClick(View view) {
        DownloadWebPageTask task = new DownloadWebPageTask();
        task.execute(new String[] { "http://192.168.2.200/cgi-bin/i2c.cgi" });

    }

    // Menu desactivado temporalmente
    /*
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    } */
}</pre>

### activity_main.xml

<pre class="lang:xhtml decode:true "><RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

    <Button
        android:id="@+id/readWebpage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="onClick"
        android:text="Load Webpage" >
    </Button>

    <TextView
        android:id="@+id/TextView01"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text=""
        android:layout_below="@+id/readWebpage">
    </TextView>
</RelativeLayout></pre>

## Documentación consultada

  * [[1]](https://developer.android.com/sdk/index.html) Descarga y configuración de Android Studio
  * [[2]](http://www.vogella.com/tutorials/AndroidBackgroundProcessing/article.html) AsyncTask
  * [[3]](https://developer.android.com/reference/android/os/AsyncTask.html) Documentación sobre la clase AsyncTask
  * [[4]](http://www.softwarepassion.com/android-series-get-post-and-multipart-post-requests/) Android GET request
  * [[5]](https://developer.android.com/reference/org/apache/http/package-summary.html) Documentación sobre la librería HTTP de Android
  * [[6]](http://www.sgoliver.net/blog/curso-de-programacion-android/indice-de-contenidos/) Curso de Programación Android
  * [7] LEE, Wei-Meng (2012) Beginning Android 4 application development. Indianapolis, IN. Willey.
  * <a href="http://media.gizmodo.co.uk/wp-content/uploads/2013/01/Android-Happy2.jpg" data-rel="lightbox-1" title="">8</a> Imagen Android

&nbsp;