---
id: 358
title: Instalación y configuración de Sublime Text en Linux para RoR
date: 2014-09-05T15:59:16+00:00
author: eodos
layout: post
guid: http://eodos.net/?p=358
permalink: /desarrollo/sublime-test-en-linux-y-ror
categories:
  - Desarrollo
tags:
  - erb
  - Linux
  - rails
  - ror
  - ruby
  - ruby on rails
  - rubytest
  - sublime
  - sublime text
  - sublime text 2
---
<a href="https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/791px-Ruby_on_Rails.svg_.png" data-rel="lightbox-0" title=""><img class=" wp-image-366 aligncenter" src="https://i1.wp.com/eodos.net/wp-content/uploads/2014/09/791px-Ruby_on_Rails.svg_-231x300.png?resize=142%2C184" alt="rails" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/791px-Ruby_on_Rails.svg_.png?resize=231%2C300&ssl=1 231w, https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/791px-Ruby_on_Rails.svg_.png?resize=620%2C802&ssl=1 620w, https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/791px-Ruby_on_Rails.svg_.png?w=791&ssl=1 791w" sizes="(max-width: 142px) 100vw, 142px" data-recalc-dims="1" /></a>

**Sublime Text** es un potente editor de código multiplataforma desarrollado en python. Vamos a ver como instalarlo en cualquier distribución de Linux (32 o 64 bits) e instalaremos algunos temas y plugins.

En primer lugar descargamos la útima versión de **Sublime Text 2** correspondiente a nuestro entorno desde la [web oficial](http://sublimetext.com/2) y lo descomprimimos.

<pre class="lang:sh decode:true">$ wget http://c758482.r82.cf2.rackcdn.com/Sublime%20Text%202.0.2%20x64.tar.bz2
$ tar -xjvf Sublime\ Text\ 2.0.2\ x64.tar.bz2</pre>

A continuación vamos a mover la carpeta descomprimida a _/opt/_ y crearemos un enlace simbólico para poder abrir **Sublime** con el alias _sublime_.

<pre class="lang:sh decode:true">$ sudo mv Sublime\ Text\ 2 /opt/
$ sudo ln -s /opt/Sublime\ Text\ 2/sublime_text /usr/bin/sublime
</pre>

A continuación vamos a realizar algunas configuraciones básicas. Abrimos el programa con el comando **sublime** y editamos:

> View > Hide Minimap
  
> View > Layout > Columns: 2

Para que aparezca la _Side Bar_ sobre nuestro directorio de trabajo, nos dirigimos a él con el comando **cd** y abrimos sublime con **sublime .**

&nbsp;

## Instalar Package Control

**Package Control** es un plugin de **Sublime Text** que permite instalar paquetes muy fácilmente. Para instalarlo en **Sublime Text 2**, abrimos una terminal con _ctrl+'_ o desde _View > Show Console_ y pegamos el siguiente texto:

> import urllib2,os,hashlib; h = &#8216;7183a2d3e96f11eeadd761d777e62404' + &#8216;e330c659d4bb41d3bdf022e94cab3cd0'; pf = &#8216;Package Control.sublime-package'; ipp = sublime.installed\_packages\_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install\_opener( urllib2.build\_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( &#8216;http://sublime.wbond.net/' + pf.replace(&#8216; &#8216;, &#8216;%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), &#8216;wb' ).write(by) if dh == h else None; print(&#8216;Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else &#8216;Please restart Sublime Text to finish installation')

&nbsp;

## Esquema de colores Railcast

Vamos a instalar el esquema de colores _RailCast_.

En primer lugar nos dirigimos a un directorio temporal y clonamos un repositorio que contiene los archivos.

<pre class="lang:sh decode:true">$ cd /tmp
$ git clone https://github.com/mhartl/rails_tutorial_sublime_text.git
$ cp -r rails_tutorial_sublime_text/* ~/.config/sublime-text-2/Packages/User/</pre>

Ahora podemos dirigirnos a _Preferences > Color Scheme > User_ y activar **Railcasts**.

&nbsp;

## Sintaxis para los archivos Sass

Los archivos **sass** son una mejora de los archivos CSS que traen muchos extras y se usan para realizar el diseño de las aplicaciones en **RoR**.

Para tener el subrayado de su sintaxis vamos a _Preferences > Package Control_ seleccionamos _Install Package_ y buscamos **Sass**.

&nbsp;

## Snipets

Para usar los Snipets de Rails, volvemos a clonar un repositorio:

<pre class="lang:sh decode:true ">$ cd ~/.config/sublime-text-2/Packages/User/
$ git clone https://github.com/mhartl/rails_tutorial_snippets.git RailsTutorial</pre>

&nbsp;

## Auto completado de sintaxis

Para tener un completado de sintaxis alternativo al que trae **Sublime**vamos a _Preferences > Package Control_ seleccionamos _Install Package_ y buscamos el paquete **Alternative Autocompletion**.

&nbsp;

## SublimeERB

Este paquete nos va a permitir añadir código **Ruby** en los archivos **html** seleccionando el texto de código que queramos y pulsado _ctrl+shift+._

<a href="https://i1.wp.com/eodos.net/wp-content/uploads/2014/09/erb.gif" data-rel="lightbox-1" title=""><img class="size-medium wp-image-360 aligncenter" src="https://i0.wp.com/eodos.net/wp-content/uploads/2014/09/erb-300x177.gif?resize=300%2C177" alt="erb" data-recalc-dims="1" /></a>

En primer lugar vamos a _Preferences > Package Control_ seleccionamos _Install Package_ y buscamos el paquete **SublimeERB**.

Para añadir el atajo de teclado, vamos a _Preferences > Key Bindings - User_ y pegamos el siguiente texto:

<pre class="lang:python decode:true ">[
    { "keys": ["ctrl+shift+."], "command": "erb" }
]</pre>

&nbsp;

Ahora podemos seleccionar un texto y usar _ctrl+shift+._ para alternar entre etiquetas **ERB**.

## Ruby Tests

Este paquete nos va a permitir correr:

  * tests unitarios (un solo test o los correspondientes a un archivo)
  * tests cucumber
  * rspec

<p style="text-align: center;">
  <a href="https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/ruby_tests.png" data-rel="lightbox-2" title=""><img class="alignnone size-medium wp-image-361" src="https://i1.wp.com/eodos.net/wp-content/uploads/2014/09/ruby_tests-300x221.png?resize=300%2C221" alt="ruby_tests" srcset="https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/ruby_tests.png?resize=300%2C221&ssl=1 300w, https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/ruby_tests.png?resize=620%2C458&ssl=1 620w, https://i2.wp.com/eodos.net/wp-content/uploads/2014/09/ruby_tests.png?w=892&ssl=1 892w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>
</p>

Instalamos el paquete con _Package Manager_. Vamos a _Preferences > Package Control_ seleccionamos _Install Package_ y buscamos el paquete **RubyTest**.

Vamos a configurarlo para que detecte **rvm**, **bundle** o **rbenv**, en el caso de que los tengamos instalados. Para ello vamos a _Preferences > Package Settings > Ruby Test > Settings - Default_ y modificamos las variables:

  * "check\_for\_rbenv": true,
  * "check\_for\_rvm": true,
  * "check\_for\_bundler": true,

Comandos:

  * _Ctrl+Shift+R_: ejecuta un test
  * _Ctrl+Shift+T_: ejecuta todos los test correspondiente al fichero que estamos viendo
  * _Ctrl+Shift+E_: ejecuta el último test
  * _Ctrl+Shift+X_: muestra el panel de los test
  * _Alt+Shift+V_: comprueba la sintaxis RB y ERB
  * _Ctrl+Shift+R_: corre un test