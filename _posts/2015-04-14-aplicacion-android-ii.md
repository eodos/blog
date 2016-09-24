---
id: 519
title: 'PFG - Aplicación Android II'
date: 2015-04-14T17:05:11+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=519
permalink: /proyectos/aplicacion-android-ii
categories:
  - Proyectos
tags:
  - android
  - pfg
  - proyecto
  - raspberry
  - tfg
---
## Novedades

Esta versión de la aplicación Android cuenta con las siguientes mejoras:<img class="  wp-image-536 alignright" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura.png?resize=330%2C544" alt="Captura" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura.png?w=565&ssl=1 565w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura.png?resize=182%2C300&ssl=1 182w" sizes="(max-width: 330px) 100vw, 330px" data-recalc-dims="1" />

  * Alerta cuando intentas realizar peticiones a la Raspberry Pi sin estar conectado a una red WiFi.
  * Alerta cuando intentas realizar una petición y no se obtiene respuesta en menos de _T_ segundos.
  * **Toast** que te avisa de que tu petición se ha realizado correctamente.
  * Ahora la aplicación tiene dos pestañas (**ActionBar.Tab**) que contienen **fragments**, uno para las configuraciones y datos generales y otra para las avanzadas.
  * Se ha cambiado el tema a **Holo Light**.
  * Se ha corregido la aplicación para que no pierda los datos que esta mostrando al cambiar la orientación del dispositivo.
  * Se ha añadido el icono de la aplicación en sus distintas resoluciones y ahora se muestra en la **ActionBar**.
  * Ahora se reciben los datos en un objeto **JSON** y se muestran en unos campos **TextView**.
  * Ahora se pueden modificar algunas variables de prueba desde la aplicación.

&nbsp;

## ToDo

  * Realizar todas las peticiones de lectura/escritura al mismo tiempo marcando con un flag las variables que hayan sido actualizadas y deban enviarse al servidor.
  * Implementar **Swipe Views**

&nbsp;

## Código

**Se procede a detallar el código utilizado:**

### AndroidManifest.xml

<pre class="lang:java decode:true"><?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="net.eodos.controlderiego" >

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >

        <activity
            android:name=".MainActivity"
            android:label="@string/app_name"
            android:configChanges="keyboardHidden|orientation|screenSize" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest></pre>

En primer lugar se han añadido permisos a la aplicación para consultar el estado de la red y así poder comprobar que se encuentra conectado a una red WiFi local.

<pre class="lang:java decode:true"><uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /></pre>

Para que la aplicación no borre el contenido de los **TextView** al cambiar la orientación del dispositivo, se añade la siguiente linea.

<pre class="lang:sh mark:3 decode:true"><activity
            ...
            android:configChanges="keyboardHidden|orientation|screenSize" >
            ...
        </activity></pre>

&nbsp;

Se ha modificado el tema de la aplicación a **Holo Light**, para ello como vemos en el _Manifest_ hay que editar el archivo _@style/AppTheme_ como se muestra a continucación.

<pre class="lang:java decode:true "><application
        ...
        android:theme="@style/AppTheme" ></pre>

### styles.xml

<pre class="lang:xhtml mark:4 decode:true"><resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="android:Theme.Holo.Light">
        <!-- Customize your theme here. -->
    </style>

</resources></pre>

&nbsp;

### MainActivity.java

<pre class="lang:java decode:true">package net.eodos.controlderiego;

import android.app.ActionBar;
import android.os.Bundle;

import android.app.Fragment;
import android.app.Activity;

public class MainActivity extends Activity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //Obtenemos una referencia a la actionbar
        ActionBar abar = getActionBar();

        //Establecemos el modo de navegación por pestañas
        if (abar != null) {
            abar.setNavigationMode(
                    ActionBar.NAVIGATION_MODE_TABS);
        }

        //Ocultamos si queremos el título de la actividad
        //abar.setDisplayShowTitleEnabled(false);

        //Creamos las pestañas
        ActionBar.Tab tab1 =
                abar.newTab().setText(getString(R.string.tab_1));

        ActionBar.Tab tab2 =
                abar.newTab().setText(getString(R.string.tab_2));

        //Creamos los fragments de cada pestaña
        Fragment tab1frag = new MainVariablesFragment();
        Fragment tab2frag = new AdvancedVariablesFragment();

        //Asociamos los listener a las pestañas
        tab1.setTabListener(new TabListener(tab1frag));
        tab2.setTabListener(new TabListener(tab2frag));

        //Añadimos las pestañas a la action bar
        abar.addTab(tab1);
        abar.addTab(tab2);
    }
}</pre>

Al contrario que en la versión anterior, se ha definido la clase como **Activity** en lugar de **ActionBarActivity**. Esto hace necesario crear la **ActionTab** y situar ahí las dos pestañas correspondientes a los dos fragments que queremos mostrar.

### activity_main.xml

El layout correspondiente a esta actividad se compone únicamente de un **LinearLayout** que ocupa toda la pantalla y donde se alojarán los **fragments**.

<pre class="lang:xhtml decode:true "><LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/contenedor"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity" >

</LinearLayout></pre>

## TabListener.java

<pre class="lang:java decode:true ">package net.eodos.controlderiego;

import android.app.ActionBar;
import android.app.Fragment;
import android.app.FragmentTransaction;
import android.util.Log;

public class TabListener implements ActionBar.TabListener {

    private Fragment fragment;

    public TabListener(Fragment fg) {
        this.fragment = fg;
    }

    @Override
    public void onTabReselected(ActionBar.Tab tab, FragmentTransaction ft) {}

    @Override
    public void onTabSelected(ActionBar.Tab tab, FragmentTransaction ft) {
        ft.replace(R.id.contenedor, fragment);
    }

    @Override
    public void onTabUnselected(ActionBar.Tab tab, FragmentTransaction ft) {
        ft.remove(fragment);
    }
}
</pre>

Esta clase se encarga de gestionar la visibilidad de los **fragments** cuando cambia entre las pestañas. Contiene tres métodos: uno que se usa cuando se reselecciona una pestaña activa, en cuyo caso no se hace nada; otra que se usa al seleccionar una pestaña, que carga el fragment en el layout definido anteriormente; y por último un método quitar el fragment del layout cuando ya no está activa esa pestaña.

### MainVariablesFragment.java

<pre class="lang:java decode:true">package net.eodos.controlderiego;

import android.app.AlertDialog;
import android.app.Fragment;
import android.content.Context;
import android.content.DialogInterface;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.AsyncTask;
import android.os.Bundle;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import org.json.JSONException;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.HashMap;

public class MainVariablesFragment extends Fragment {

    private static final String DEBUG_TAG = "HttpDebug";
    public TextView countField;
    public TextView tempField;
    public EditText xField;
    public Button buttonRead;
    public Button buttonX;

    // JSON Node names
    private static final String TAG_COUNT = "COUNT";
    private static final String TAG_TEMP = "TEMP";
    private static final String TAG_X = "X";

    // Hashmap
    HashMap<String, String> data;

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

        // Inflate the layout for this fragment
        View view = inflater.inflate(R.layout.main_variables_fragment, container, false);

        // Creamos el array de variables de lectura
        data = new HashMap<>(); // <string, string>

        //Referencia a controles de la interfaz
        countField = (TextView) view.findViewById(R.id.count);
        tempField = (TextView) view.findViewById(R.id.temperature);
        xField = (EditText) view.findViewById(R.id.x);
        buttonRead = (Button) view.findViewById(R.id.buttonRefresh);
        buttonX = (Button) view.findViewById(R.id.xSend);

        buttonRead.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                readVariables();
            }
        });

        buttonX.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                writeXVariable();
            }
        });

        return view;
    }

    @Override
    public void onActivityCreated(Bundle state) {
        super.onActivityCreated(state);
    }

    // When user clicks button, calls AsyncTask.
    // Before attempting to fetch the URL, makes sure that there is a network connection.
    public void readVariables() {
        String[] vars = {"COUNT","TEMP","X"};
        String URL = composeReadURL(vars);
        if (checkConnection()) {
            Log.d(DEBUG_TAG, "Connecting to: " + URL);
            new downloadVariablesTask().execute(URL);
        }
    }

    public void writeXVariable() {
        String var = "X";
        writeVariable(var, xField);
    }

    public void writeVariable(String var, EditText editText) {
        String value = editText.getText().toString();
        String URL = composeWriteURL(var, value);
        if (checkConnection()) {
            Log.d(DEBUG_TAG, "Connecting to: " + URL);
            new writeVariableTask().execute(URL);
        }
    }

    public String composeReadURL(String[] vars) {
        String URL = "http://192.168.2.200/cgi-bin/read.cgi?";
        for (int i=0; i<vars.length; i++) {
            if (i==0) {
                URL = URL + "var=" + vars[i];
            }
            else {
                URL = URL + "&var=" + vars[i];
            }
        }
        return URL;
    }

    public String composeWriteURL(String var, String value) {
        return "http://192.168.2.200/cgi-bin/write.cgi?"+var+"="+value;
    }

    public boolean checkConnection() {
        ConnectivityManager connMgr = (ConnectivityManager)
                getActivity().getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo networkInfo = connMgr.getNetworkInfo(ConnectivityManager.TYPE_WIFI);
        if (networkInfo != null && networkInfo.isConnected()) {
            return true;
        } else {
            showWifiDialog();
            return false;
        }
    }

    // Uses AsyncTask to create a task away from the main UI thread. This task takes a
    // URL string and uses it to create an HttpUrlConnection. Once the connection
    // has been established, the AsyncTask downloads the contents of the webpage as
    // an InputStream. Finally, the InputStream is converted into a string, which is
    // displayed in the UI by the AsyncTask's onPostExecute method.
    public class downloadVariablesTask extends AsyncTask<String, Void, HashMap> {
        @Override
        protected HashMap doInBackground(String... params) {
            // params comes from the execute() call: params[0] is the url.
            try {
                return downloadUrl(params[0],true);
            } catch (IOException e) {
                getActivity().runOnUiThread(new Runnable() {
                    public void run() {
                        showUnableToConnectDialog();
                    }
                });
                return null;
            }
        }
        // onPostExecute displays the results of the AsyncTask.
        @Override
        protected void onPostExecute(HashMap data) {
            if (data != null) {
                String count = (String) data.get(TAG_COUNT);
                String temperature = (String) data.get(TAG_TEMP);
                String x = (String) data.get(TAG_X);
                countField.setText(count);
                tempField.setText(temperature);
                xField.setText(x);
            }
        }
    }

    public class writeVariableTask extends AsyncTask<String, Void, HashMap> {
        @Override
        protected HashMap doInBackground(String... params) {
            // params comes from the execute() call: params[0] is the url.
            try {
                return downloadUrl(params[0],false);
            } catch (IOException e) {
                getActivity().runOnUiThread(new Runnable() {
                    public void run() {
                        showUnableToConnectDialog();
                    }
                });
                return null;
            }
        }
    }

    // Given a URL, establishes an HttpUrlConnection and retrieves
    // the web page content as a InputStream, which it returns as
    // a string./*
    public HashMap downloadUrl(String myurl, boolean read) throws IOException {
        InputStream is = null;

        try {
            URL url = new URL(myurl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setReadTimeout(3000);
            conn.setConnectTimeout(5000);
            conn.setRequestMethod("GET");
            conn.setDoInput(true);
            // Starts the query
            conn.connect();
            int response = conn.getResponseCode();
            Log.d(DEBUG_TAG, "The response is: " + response);

            // Show toast
            getActivity().runOnUiThread(new Runnable() {
                public void run() {
                    showSuccessToast();
                }
            });

            is = conn.getInputStream();

            if (read) {
                // Convert the InputStream into a HashMap
                return parseJSON(is);
            }

            else {
                return null;
            }

            // Makes sure that the InputStream is closed after the app is
            // finished using it.
        } finally {
            if (is != null) {
                is.close();
            }
        }
    }

    public HashMap parseJSON(InputStream stream) throws IOException {

        try {
            // Convert InputStream to a String
            BufferedReader r = new BufferedReader(new InputStreamReader(stream));
            StringBuilder result = new StringBuilder();
            String line;
            while ((line = r.readLine()) != null) {
                result.append(line);
            }

            // Convert the String to a HashMap object
            JSONObject serverJSON = new JSONObject(result.toString());
            String count = serverJSON.getString(TAG_COUNT);
            String temp = serverJSON.getString(TAG_TEMP);
            String x = serverJSON.getString(TAG_X);

            // New temp HashMap
            HashMap<String, String> data = new HashMap<>();

            // Adding each child node to Hashmap key => value
            data.put(TAG_COUNT, count);
            data.put(TAG_TEMP, temp);
            data.put(TAG_X, x);

            // Return the Hashmap
            return data;

        } catch (JSONException e) {
            e.printStackTrace();
            return null;
        }
    }

    public void showWifiDialog() {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
        builder.setMessage(getString(R.string.missingWifi))
                .setCancelable(false)
                .setNegativeButton(getString(R.string.ok), new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.cancel();
                    }
                });
        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }

    public void showUnableToConnectDialog() {
        AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
        builder.setMessage(getString(R.string.unabletoconnect))
                .setCancelable(false)
                .setNegativeButton(getString(R.string.ok), new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.cancel();
                    }
                });
        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }

    private void showSuccessToast() {
        Context context = getActivity().getApplicationContext();
        CharSequence text = context.getString(R.string.success);
        int duration = Toast.LENGTH_SHORT;

        Toast toast = Toast.makeText(context, text, duration);
        toast.show();
    }
}</pre>

En este **fragment** es en el que se alojarán los datos y configuraciones principales de la aplicación.

En primer lugar se definen algunas variables que se van a utilizar después. Entre ellas un **HashMap** (par de valores) donde se guardarán los datos recibidos en JSON.

<pre class="lang:java decode:true">private static final String DEBUG_TAG = "HttpDebug";
public TextView countField;
public TextView tempField;
public EditText xField;
public Button buttonRead;
public Button buttonX;
 
// JSON Node names
private static final String TAG_COUNT = "COUNT";
private static final String TAG_TEMP = "TEMP";
private static final String TAG_X = "X";
 
// Hashmap
HashMap<String, String> data;</pre>

Después se asocian estas variables a los controles de la vista de la actividad y se definen las acciones a invocar cuando se activan los botones de la vista. Al trabajar con fragments es necesario definirlo en la clase java y no en el layout xml ya que un fragment puede estar activo en distintas actividades y necesita conocer la vista en la que actuar.

<pre class="lang:java decode:true">//Referencia a controles de la interfaz
countField = (TextView) view.findViewById(R.id.count);
tempField = (TextView) view.findViewById(R.id.temperature);
xField = (EditText) view.findViewById(R.id.x);
buttonRead = (Button) view.findViewById(R.id.buttonRefresh);
buttonX = (Button) view.findViewById(R.id.xSend);
 
buttonRead.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        readVariables();
    }
});
 
buttonX.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        writeXVariable();
    }
});</pre>

<img class="  wp-image-538 alignright" src="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura3.png?resize=330%2C544" alt="Captura3" srcset="https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura3.png?w=563&ssl=1 563w, https://i1.wp.com/eodos.net/wp-content/uploads/2015/04/Captura3.png?resize=182%2C300&ssl=1 182w" sizes="(max-width: 330px) 100vw, 330px" data-recalc-dims="1" />El método **public void readVariables()** se invoca al activar el botón **Refresh Variables**. Este método llama en primer lugar a otro que crea la URL a la que realizar la petición a partir de las variables que se quieren obtener mediante el método **string composeReadURL(string vars)**, en segundo lugar comprueba que el dispositivo esté conectado a una red WiFi mediante el método **bool checkConnection()**, y por último instancia la clase **downloadVariablesTask()** que realizará todo el proceso de petición y parseo de los datos en un hilo que corra en segundo plano, además de mostrar estos datos en la vista.

<pre class="lang:java decode:true">// When user clicks button, calls AsyncTask.
// Before attempting to fetch the URL, makes sure that there is a network connection.
public void readVariables() {
    String[] vars = {"COUNT","TEMP","X"};
    String URL = composeReadURL(vars);
    if (checkConnection()) {
        Log.d(DEBUG_TAG, "Connecting to: " + URL);
        new downloadVariablesTask().execute(URL);
    }
}</pre>

El método **public void writeXVariables()** llama al método **void writeVariable(String var, EditText editText)** y le pasa como argumento el nombre de la variable que quiere enviar y el elemento de la vista en el que se encuentra.

<pre class="lang:sh decode:true ">public void writeXVariable() {
    String var = "X";
    writeVariable(var, xField);
}</pre>

El método **public void writeVariable(String var, EditText editText)** extrae la variable del elemento de la interfaz, llama a **String composeWriteURL(String var, String value)** para obtener la URL a la que realizar la petición, comprueba que tiene conexión WiFi con **checkConnection()** e instancia la clase **writeVariableTask()** que se encargará de realizar la petición.

<pre class="lang:java decode:true ">public void writeVariable(String var, EditText editText) {
    String value = editText.getText().toString();
    String URL = composeWriteURL(var, value);
    if (checkConnection()) {
        Log.d(DEBUG_TAG, "Connecting to: " + URL);
        new writeVariableTask().execute(URL);
    }
}</pre>

El método **public String composeReadURL(String[] vars)** recibe como argumento un array con todas las variables que quiere actualizar y se encarga de construir y devolver una URL en la que se incluyan.

<pre class="lang:sh decode:true ">public String composeReadURL(String[] vars) {
    String URL = "http://192.168.2.200/cgi-bin/read.cgi?";
    for (int i=0; i<vars.length; i++) {
        if (i==0) {
            URL = URL + "var=" + vars[i];
        }
        else {
            URL = URL + "&var=" + vars[i];         
        }
    }
    return URL;
}</pre>

El método **public String composeWriteURL(String var, String value)** construya la url a la que realizar la petición de escritura añadiendo el nombre de la variable y su valor para que al realizar la petición GET el script CGI reciba estos datos.

<pre class="lang:java decode:true ">public String composeWriteURL(String var, String value) {
    return "http://192.168.2.200/cgi-bin/write.cgi?"+var+"="+value;
}</pre>

El método **public boolean checkConnection()** comprueba en el contexto de la actividad si está activa la conectividad WiFi. En caso afirmativo devuelve **true** y en caso contrario devuelve **false** y muestra una **Alerta** indicándolo.

<pre class="lang:js decode:true">public boolean checkConnection() {
    ConnectivityManager connMgr = (ConnectivityManager)
            getActivity().getSystemService(Context.CONNECTIVITY_SERVICE);
    NetworkInfo networkInfo = connMgr.getNetworkInfo(ConnectivityManager.TYPE_WIFI);
    if (networkInfo != null && networkInfo.isConnected()) {
        return true;
    } else {
        showWifiDialog();
        return false;
    }
}</pre>

La clase **public class downloadVariablesTask** crea una tarea en segundo plano mediante **AsyncTask** e intenta realizar la petición mediante el método **downloadUrl** que devuelve un HashMap con los datos recibidos ya parseados. Si han pasado más de _T_ segundos y no recibe respuesta, aparece una **Alerta** indicando que no se puede conectar al dispositivo. Si todo va bien, muestra estos datos en los controles de la interfaz correspondientes.

Para poder mostrar la alerta en esta clase es necesario indicar que se invoque al método de la alerta en el thread principal de la aplicación con **runOnUiThread**.

<pre class="lang:java decode:true ">// Uses AsyncTask to create a task away from the main UI thread. This task takes a
// URL string and uses it to create an HttpUrlConnection. Once the connection
// has been established, the AsyncTask downloads the contents of the webpage as
// an InputStream. Finally, the InputStream is converted into a string, which is
// displayed in the UI by the AsyncTask's onPostExecute method.
public class downloadVariablesTask extends AsyncTask<String, Void, HashMap> {
    @Override
    protected HashMap doInBackground(String... params) {
        // params comes from the execute() call: params[0] is the url.
        try {
            return downloadUrl(params[0],true);
        } catch (IOException e) {
            getActivity().runOnUiThread(new Runnable() {
                public void run() {
                    showUnableToConnectDialog();
                }
            });
            return null;
        }
    }
    // onPostExecute displays the results of the AsyncTask.
    @Override
    protected void onPostExecute(HashMap data) {
        if (data != null) {
            String count = (String) data.get(TAG_COUNT);
            String temperature = (String) data.get(TAG_TEMP);
            String x = (String) data.get(TAG_X);
            countField.setText(count);
            tempField.setText(temperature);
            xField.setText(x);
        }
    }
}</pre>

<img class="  wp-image-539 alignright" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura4.png?resize=328%2C540" alt="Captura4" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura4.png?w=562&ssl=1 562w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura4.png?resize=182%2C300&ssl=1 182w" sizes="(max-width: 328px) 100vw, 328px" data-recalc-dims="1" />La clase **public class writeVariablesTask** realiza lo mismo pero para las peticiones de escritura, se ha separado en otra clase ya que esta no debe realizar ninguna tarea en _onPostExecute_ y por tanto resulta más sencillo hacerlo de esta manera indicando con un **false** al método **downloadUrl** que no se quiere recibir ningún dato, por lo que este último no realizará el parseo de los datos recibidos al ser inexistentes.

<pre class="lang:java decode:true ">public class writeVariableTask extends AsyncTask<String, Void, HashMap> {
    @Override
    protected HashMap doInBackground(String... params) {
        // params comes from the execute() call: params[0] is the url.
        try {
        return downloadUrl(params[0],false);
        } catch (IOException e) {
            getActivity().runOnUiThread(new Runnable() {
                public void run() {
                    showUnableToConnectDialog();
                }
            });
            return null;
        }
    }
}</pre>

El método **public HashMap downloadUrl(String myurl, boolean read)** se encarga de establecer la conexión con la URL destino, abrir un _stream_ de datos e invocar al método **paseJSON**para que obtenga los datos del stream cuando estamos realizando una petición de lectura y los convierta a un HashMap, el cual devuelve. Muestra un **toast** cuando la petición se realiza correctamente.

<pre class="lang:less decode:true ">// Given a URL, establishes an HttpUrlConnection and retrieves
// the web page content as a InputStream, which it returns as
// a string./*
public HashMap downloadUrl(String myurl, boolean read) throws IOException {
    InputStream is = null;

    try {
        URL url = new URL(myurl);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setReadTimeout(3000);
        conn.setConnectTimeout(5000);
        conn.setRequestMethod("GET");
        conn.setDoInput(true);
        // Starts the query
        conn.connect();
        int response = conn.getResponseCode();
        Log.d(DEBUG_TAG, "The response is: " + response);

        // Show toast
        getActivity().runOnUiThread(new Runnable() {
            public void run() {
                showSuccessToast();
            }
        });

        is = conn.getInputStream();
  
        if (read) {
            // Convert the InputStream into a HashMap
            return parseJSON(is);
        }

        else {
            return null;
        }

        // Makes sure that the InputStream is closed after the app is
        // finished using it.
    } finally {
        if (is != null) {
            is.close();
        }
    }
}</pre>

El método **public HashMap parseJSON(InputStream stream)** recibe como argumento un stream de datos, los convierte a String y luego a un objeto HashMap usando la librería **org.json.JSONObject** para decodificar los datos. Hace uso de las variables definidas globalmente al principio del archivo para extraer del objeto cada par de valores.

<pre class="lang:java decode:true ">public HashMap parseJSON(InputStream stream) throws IOException {

    try {
        // Convert InputStream to a String
        BufferedReader r = new BufferedReader(new InputStreamReader(stream));
        StringBuilder result = new StringBuilder();
        String line;
            while ((line = r.readLine()) != null) {
            result.append(line);
        }

        // Convert the String to a HashMap object
        JSONObject serverJSON = new JSONObject(result.toString());
        String count = serverJSON.getString(TAG_COUNT);
        String temp = serverJSON.getString(TAG_TEMP);
        String x = serverJSON.getString(TAG_X);

        // New temp HashMap
        HashMap<String, String> data = new HashMap<>();

        // Adding each child node to Hashmap key => value
        data.put(TAG_COUNT, count);
        data.put(TAG_TEMP, temp);
        data.put(TAG_X, x);

        // Return the Hashmap
        return data;

    } catch (JSONException e) {
        e.printStackTrace();
        return null;
    }
}</pre>

Los siguientes métodos se encargan de construir y mostrar las alertas invocadas cuando no hay conectividad WiFi o cuando no se puede conectar con el servidor.

<pre class="lang:java decode:true ">public void showWifiDialog() {
    AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
    builder.setMessage(getString(R.string.missingWifi))
            .setCancelable(false)
            .setNegativeButton(getString(R.string.ok), new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    dialog.cancel();
                }
            });
    AlertDialog alertDialog = builder.create();
    alertDialog.show();
}

public void showUnableToConnectDialog() {
    AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
    builder.setMessage(getString(R.string.unabletoconnect))
            .setCancelable(false)
            .setNegativeButton(getString(R.string.ok), new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    dialog.cancel();
                }
            });
    AlertDialog alertDialog = builder.create();
    alertDialog.show();
}</pre>

Por último el método **private void showSuccessToast()** crea la pequeña notificación que se muestra en la parte inferior de la pantalla cuando  se realizar con éxito una petición de lectura o escritura.

<pre class="lang:java decode:true">private void showSuccessToast() {
    Context context = getActivity().getApplicationContext();
    CharSequence text = context.getString(R.string.success);
    int duration = Toast.LENGTH_SHORT;

    Toast toast = Toast.makeText(context, text, duration);
    toast.show();
}</pre>

<img class="  wp-image-537 alignright" src="https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura2.png?resize=331%2C519" alt="Captura2" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura2.png?w=564&ssl=1 564w, https://i2.wp.com/eodos.net/wp-content/uploads/2015/04/Captura2.png?resize=191%2C300&ssl=1 191w" sizes="(max-width: 331px) 100vw, 331px" data-recalc-dims="1" />\### main\_variables\_fragment.xml

En este layout se ubican los controles de interfaz utilizados en la clase **MainVariablesFragment.java**, mostrando algunas variables de ejemplo. Utiliza un **RelativaLayout** como base.

<pre class="lang:xhtml decode:true"><RelativeLayout

    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:id="@+id/layout">

    <!-- Refresh button -->
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_refresh_variables"
        android:id="@+id/buttonRefresh"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true" />

    <!-- Count Label -->
    <TextView
        android:id="@+id/countLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/count_label"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true" />

    <!-- Count Value -->
    <TextView
        android:id="@+id/count"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:layout_marginStart="5dp"
        android:text=""
        android:layout_alignTop="@id/countLabel"
        android:layout_toRightOf="@id/countLabel"
        android:layout_toEndOf="@id/countLabel" />

    <!-- Temperature Label -->
    <TextView
        android:id="@+id/temperatureLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/temperature_label"
        android:layout_below="@+id/countLabel"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_marginTop="34dp" />

    <!-- Temperature Value -->
    <TextView
        android:id="@+id/temperature"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:layout_marginStart="5dp"
        android:text=""
        android:layout_alignBottom="@id/temperatureLabel"
        android:layout_toRightOf="@id/temperatureLabel"
        android:layout_toEndOf="@id/temperatureLabel" />

    <!-- x Label -->
    <TextView
        android:id="@+id/xLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/X_label"
        android:layout_below="@id/temperatureLabel"
        android:layout_marginTop="30dp"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true" />

    <EditText
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:inputType="numberDecimal"
        android:ems="3"
        android:id="@+id/x"
        android:layout_alignBottom="@id/xLabel"
        android:layout_toRightOf="@id/xLabel"
        android:layout_toEndOf="@id/xLabel"
        android:layout_marginLeft="5dp"
        android:layout_marginStart="5dp"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/save_button"
        android:id="@+id/xSend"
        android:layout_alignBottom="@+id/x"
        android:layout_toRightOf="@+id/x"
        android:layout_toEndOf="@+id/x"/>

</RelativeLayout></pre>

### AdvancedVariablesFragment.java

Este fragment se utilizará para las opciones avanzadas de la apliación.

<pre class="lang:java decode:true">package net.eodos.controlderiego;

import android.app.Fragment;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class AdvancedVariablesFragment extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return inflater.inflate(R.layout.advanced_variables_fragment, container, false);
    }
}
</pre>

### advanced\_variables\_fragment.xml

El layout correspondiente a la segunda pestaña. Únicamente contiene un **CheckBox** de momento para probar el correcto funcionamiento de las pestañas.

<pre class="lang:xhtml decode:true "><?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="New CheckBox"
        android:id="@+id/checkBox" />
</LinearLayout></pre>

&nbsp;

## Referencias

  * [[1]](http://www.androidhive.info/2012/01/android-json-parsing-tutorial/) JSON Parsing
  * [[2]](https://developer.android.com/training/basics/network-ops/connecting.html) API para las operaciones de networking
  * [[3]](https://developer.android.com/reference/java/util/HashMap.html) Uso de un HashMap
  * [[4]](https://developer.android.com/guide/topics/ui/dialogs.html) Dialogs
  * [[5]](http://android-developers.blogspot.com.es/2009/05/painless-threading.html), [[6]](https://developer.android.com/reference/android/app/Activity.html#runOnUiThread) Run Alerts on the UI Thread
  * [[7]](https://developer.android.com/guide/topics/ui/actionbar.html), [[8]](http://www.sgoliver.net/blog/action-bar-en-android-ii/) ActionBar.Tab
  * [[9]](https://developer.android.com/reference/android/app/Fragment.html) Fragments
  * [[10]](http://stackoverflow.com/questions/8879474/textviews-text-disappearing-when-device-rotated) Orientation changes
  * [[11]](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) Toasts
  * [[12]](https://romannurik.github.io/AndroidAssetStudio/) Herramienta utilizada para crear el icono de la aplicación
