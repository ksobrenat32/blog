---
layout: post
title: "Usa GNU/Linux básico"
categories: GNU-Linux
permalink: /Usa-GNU-Linux/2
visible: 0
author:
- ksobrenatural
---
Linux es muy parecidos a otros sistemas operativos, al menos desde la superficie, si exploraste un poco el sistema te habrás dado cuenta que se tienen herramientas en común, navegador web, calendarios, relojes, configuraciones de tu equipo, etc. Pero eso es algo que tú notaras, vamos a hablar de como manejar un poco el sistema para facilitar la comprensión de esta guía.

## La terminal

El uso del emulador de terminal a muchos les parece complejo, no tener botones que picar puede ser intimidante, sin embargo es algo muy sencillo una vez que le pierdes el miedo.

Para acceder a ella simplemente en tu entorno de escritorio busca un programa que se llame terminal, en el caso de kde se llama konsole, al iniciarlo verás algo así: 

    <usuario>@<nombre_de_la_maquina>:~$

Esta es tu terminal, ¿simple, no?, bueno aquí puedes escribir órdenes y tu computadora las va a seguir, prueba con algo como 

    echo 'hola'

Echo es un programa simple, imprime en la terminal lo que tú le indiques, puedes tratar con cosas más vanidosas, prueba con 

    uname -a

Verás algo de texto, algo así como 

    Linux debian 5.10.0-6-amd64 #1 SMP Debian 5.10.28-1 (2021-04-09) x86_64 GNU/Linux

Esto es porque el programa uname te da datos sobre el sistema, el '-a' que agregamos es a lo que se le llama bandera o parámetro, son indicaciones extra para la ejecución del programa, si solo usas 'uname', te dirá algo así como:

    Linux

Bueno y te preguntarás, ¿Cómo puedo saber cuáles son las banderas?, para esto se usa el comando 'man' y posteriormente el nombre del comando, algo así como

    man uname


>En caso de que te marque un error, es posible que aún no tengas instalado man, se hace con este comando

    sudo apt install man-db -y

>Ingresas tu contraseña y listo, más adelante en la guía explicaré que significa este comando

El comando 'man' hace referencia a un manual, muchos programas cuentan con manuales escritos por los desarrolladores para que se te facilite el uso de sus programas, los manuales están escritos en inglés sin embargo siempre esta disponible internet para llevar a cabo búsquedas y traducciones. Para salir simplemente tecleas la 'q' que hace referencia a quit (salir en inglés).

Para moverte entre tus directorios (folders) puedes usar el comando 

> Puedes autocompletar automáticamente presionando la tecla de tabulador.

    cd <Nombre del directorio>

Vamos a explorar el sistema de archivos, escribe el comando

    cd /

Ahora estas en tu sistema raíz, es la base de todo tu sistema, ¿quieres saber que hay?, usa el comando

    ls

Este te imprime la lista de directorios que se encuentran donde estás, verás directorios como 'bin' que son los binarios(programas en idioma de computadora), 'etc' que es donde se guardan muchas de las configuraciones, o 'home' que es donde se guardan los archivos de los usuarios, si quieres explorar muévete con 'cd' y 'ls', para regresar al directorio superior, puedes usar

    cd ..

Que es uno de los llamados comodines o atajos.

Existen miles de comandos que en esta guía no se usaran, sin embargo otros comandos que debes saber que están ahí, son 

    cat

Que imprime un archivo, por ejemplo 

    cat /etc/hostname

Que imprime el nombre de tu máquina.

Otro importante es

    nano

Que es un editor de texto por terminal, muchas veces queremos editar un texto, pero sin todo el entorno gráfico, esto se logra con nano, prueba algo como

    nano $HOME/hola.txt

Que creara un archivo en tu carpeta de usuario llamado 'hola.txt', y te introducirá en él para editarlo, si el archivo ya existía previamente, simplemente te introducirá a modificar el archivo, para guardar los cambios se utiliza, 'control+o' y para salir es con 'control+x', si estás creando el archivo, te permitirá cambiar el nombre, tú simplemente da 'enter' para aceptar.

Bueno, pensarás que ya te deje un archivo que no sirve para nada, bueno para eso está el comando

    rm

Con este debes de tener extrema precaución porque no existe algo como una papelera, si usas rm se borrara tu archivo para siempre, así que ten cuidado. Para borrar el archivo que creamos, puedes usar:

    rm $HOME/hola.txt

Bueno ya que conoces los comando mas básicos, te toca explorar, la mayoria, si no es que todos, los comandos tienen un manual, recuerda siempre revisarlo, tal ves te puede ayudar.

## Usuario administrador o superusuario

Al igual que en otros sistemas operativos, en linux existe un usuario administrador, para ejecutar cosas como administrador se usa:

    sudo <programa>

Este comando es muy poderoso, te permite ejecutar órdenes como otro usuario, en este caso como el usuario administrador, también llamado 'root'. Algunos usos que le puedes dar es al instalar un programa o realizar modificaciones al sistema, el comando es muy útil, pero debes de tener cuidado, en muchos lugares de internet, guías o tutoriales, lo utilizan indiscriminadamente, Linux no te cuestiona lo que haces, si le dices que se borre a sí mismo, lo hará, entonces por favor, ten mucho cuidado al usar este comando, especialmente si no sabes lo que haces **Nunca copies y pegues sin antes entender**

## ¡Muy bien!

Has entendido lo básico de GNU/Linux, en realidad puede que nunca toques la terminal pues casi todo lo puedes hacer de manera gráfica, sin embargo es bueno que entiendas como manejarla pues es mucho más poderosa y rápida que hacer todo con clicks.

Puedes dejar la guía aquí, si quieres entender un poco más, pasa a la sección [Software]({{site.baseurl}}/Usa-GNU-Linux/3)