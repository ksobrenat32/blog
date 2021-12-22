---
layout: post
title: "Un servidor con linux"
categories: server
visible: 1
author:
- ksobrenatural
---

## Introducción

Después de escribir la guía para tener un servidor
 de minecraft me di cuenta que hacía falta una guía
 básica para tener un servidor seguro, así que
 aquí esta, esta es la base no solo para un servidor
 de minecraft, igual quieres mantener algunas páginas
 web o simplemente aprender.

En esta guía usaré Debian que considero es la
 distribución de GNU/Linux mejor para principiantes
 a demás de ser muy estable y requerir relativamente
 poco mantenimiento. Sin embargo es compatible con
 las derivadas como Ubuntu y sus variantes, incluso
 si tienes una interfaz gráfica, pero recuerda que
 las interfaces gráficas consumen recursos como cpu
 o ram que podrías estar utilizando en otros procesos.

## Requerimientos

- Una computadora dedicada a ser servidor o instancia
 virtual en algún proveedor como Linode, Azure, Amazon (etc).
- Otra computadora, esta puede ser la que usas normalmente,
 sin importar su sistema operativo aunque preferentemente
 instala algún GNU/Linux.
- Conexión a internet en ambas computadoras, es muy
 recomendable tener conexión por Ethernet si tienes
 físicamente el servidor.

## Instalación del servidor

Esta parte dependerá mucho, si tienes la computadora
 te tocará instalar todo por tu cuenta, si usas un
 proveedor de nube, solo escoge la última versión de
 Debian

Para instalar un sistema con interfaz gráfica puedes
 dirigirte a la guía de GNU/Linux del menú principal,
 en ella se instala kubuntu, es ubuntu con la interfaz
 gráfica de KDE, es algo pesado pero si le tienes miedo
 a la terminal puedes usar eso.

Reitero, lo mejor es instalar Debian puro, como ese es
 un tema por si solo, te dejo la [guía oficial](https://www.debian.org/releases/stable/amd64/)
 puede que parezca algo larga pero te enseñará mucho, si no
 entiendes el idioma puedes utilizar un traductor o
 buscar los términos en internet. Igualmente puedes
 buscar por tu cuenta otros recursos que te guíen
 durante la instalación del sistema. No olvides
 instalar ssh-server.

### Primer arranque

Una vez arrancado el sistema, y con ssh instalado, desde
 tu otra computadora abre una terminal, en Linux y MacOS
 es la terminal, en Windows puede ser cmd.

Tendrás que obtener la ip de la máquina nueva, si es un
 proveedor seguramente te la proporcionará, si es una
 computadora local tocará obtenerla. Puede ser a través
 del router que es lo mas sencillo o a través de algún
 programa que analice la red. Tu decide e investiga
 como hacerlo.

Una vez obtengas la ip puedes usar desde tu terminal el
 comando:

    ssh usuario@1.2.3.4

Sustituyendo `usuario` por el usuario que creaste y
 `1.2.3.4` por la ip de tu nuevo servidor, te pedirá la
 contraseña del usuario.

Una vez con acceso al servidor lo primero que tienes
 que hacer es actualizar el sistema, necesitas permiso
 root para esto, si configuraste la contraseña de root
 tendrás que logearte como root, se consigue con:

    su

Ingresas la contraseña de root y listo, si no lo
 configuraste es probable que puedas usar sudo con tu
 contraseña de usuario, en ese caso será:

    sudo apt update

### SSH

Ahora tocará configurar y reforzar lo que me parece mas
 importante, el acceso por ssh. Lo mejor es dejar de
 usar contraseñas pues alguien con suficiente tiempo
 siempre podrá adivinarla, lo que se usa son las llaves
 de ssh, estas son un par, privada y publica, la publica
 es la que tienes en los servidores y la privada es la que
 no compartes con nadie. En primer lugar salimos del
 servidor, esto se hace con:

    exit

Para crear las claves se hará  desde tu maquina aparte,
 esto sera con:

    ssh-keygen -o -a 100 -t ed25519

Esto generara una clave muy segura y corta a la vez, ahora
 copiaremos la clave generada que termina con `.pub`, esa
 es la clave publica, puedes copiarla al servidor con

    ssh-copy-id usuario@1.2.3.4

Te pedirá tu contraseña y listo, pruebala saliendo del
 servidor, y volviendo a entrar, ya no deberá de pedirte
 contraseña, si funciona es momento de desactivar el
 acceso por ssh, dentro del servidor debemos editar el
 archivo de configuración de ssh, esto se hace con:

    sudo nano /etc/ssh/sshd_config

Abriendo el archivo puedes leer todo y escoger lo que
 te convenga, lo que yo te recomiendo es seguir mi
 [archivo](https://github.com/ksobrenat32/notes/blob/main/ssh/sshd_config)
 de configuración como base, es importante recalcar
 que los que tengan `#` al inicio son las configuraciones
 que no tendrán efecto, se vuelven comentarios. De aquí
 lo que te recomiendo cambiar es la última linea, descomentar
 `AllowUsers` y cambiar username por tu nombre de usuario.

Listo, puedes reiniciar el servicio de ssh con:

    sudo systemctl restart sshd

Sin cerrar esa terminal, abre otra y prueba conectarte con ssh
 si funciona perfecto, la configuración sirve. En caso de que
 no te de acceso, regresa a la terminal abierta e intenta
 modificar las lineas, tarde o temprano podrás entrar, lo
 importante es dejar:

    # Disable password authentication and empty passwords
    PasswordAuthentication no
    PermitEmptyPasswords no
    ChallengeResponseAuthentication no

Pues estas lineas son las que prohíben entrar con contraseñas
 y admiten unicamente la clave ssh, además de estas es importante
 tener:

    # Root Login not allowed
    PermitRootLogin no

Pues esta linea desactiva completamente el acceso de root por ssh
, que es lo que mas intentan los bots.

Por ultimo, puedes cambiar el puerto de ssh, lo que hará que
 menos bots intenten entrar, cambia el valor a uno entre 1024-
65535, si tienes tu servidor en la nube, debes de recordar
 actualizar el firewall para permitir acceso a ese puerto. Ademas
 ahora para acceder al servidor, al comando de acceso le debes de
 añadir `-p puerto` por ejemplo, si escogiste el puerto 2392 sera:

    ssh -p 2392 usuario@1.2.3.4

### Firewall

Todo servidor Linux debe tener un firewall, este será el que
 impida el acceso del mundo exterior al servidor por medio de
 ciertas reglas. Si eres principiante te recomiendo usar `UFW`
 que es de los firewalls mas fáciles de administrar. Para
 instalarlo se hace con:

    sudo apt install ufw

Por default todo el que quiera acceder esta bloqueado y solo
 se permite la salida por lo que en primer lugar debemos de
 permitir el acceso por ssh. En caso de que no hayas cambiado
 el puerto por default será el puerto 22 en el protocolo tcp,
 para permitir ese acceso es con:

    sudo ufw allow 22

Una vez que se permitió el acceso por ssh ya podemos activar
 el firewall con:

    sudo ufw enable

Listo, a partir de aquí puedes abrir los puertos que ocupes
 por ejemplo 80 para http o 443 para https, si usas minecraft
 java será 25565 o bedrock es 12132, todo dependerá de lo que
 estés corriendo en el servidor.

## Listo

A partir de aquí ya puedes agregar lo que vayas necesitando, lo
 mas vulnerable que es el firewall y el acceso por ssh ya es
 más seguro que por default.

Como recomendaciones extras te recomendaría buscar como reforzar
 los servicios que estés corriendo además de estar al pendiente
 de los permisos que tiene cada programa, recuerda que siempre
 es mejor que tenga los mínimos necesarios.
