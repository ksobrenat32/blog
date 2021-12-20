---
layout: post
title: "Servidor de minecraft"
categories: games
visible: 1
author:
- ksobrenatural
---

## Introducción

A la mayoría de personas que conozco les gusta jugar
 minecraft, es un juego increíble, pero pocos saben
 lo sencillo que es correr su propio servidor.

El este post explicaré como crear un servidor de
 minecraft, usaremos el servidor modificado
 [paper](https://papermc.io/) para poder usar los
 plugins que permiten jugar juntos a java y bedrock
 aunque no hay que olvidar que el servidor sigue
 siendo de java por lo que las dinámicas del juego
 como las granjas siguen siendo de Java.

## ¿Qué es un servidor de minecraft?

Un servidor no es mas que una computadora, en este
 caso corriendo minecraft. Aunque la administración
 de un servidor parece complicada, haciéndolo bien
 desde el inicio, el resto es fácil.

El programa que correremos como servidor de minecraft
 no es el original proporcionado por mojang pues ese
 no permite el uso de plugins además de que paper
 tiene optimizaciones para volverlo mas ligero a
 cambio de unas diferencias en unas dinámicas que
 es difícil notar, en caso de que quieras una
 experiencia 100% vanilla y no necesitas compatibilidad
 con bedrock, puedes usar el ejecutable proporcionado
 por mojang.

## Requerimientos

- Una computadora con GNU/Linux donde vayas a correr
 el servidor, en este caso será en Azure\* con la
 distribución Debian 11.
- Amigos (jajaj si no para que crear un servidor).

\* Si eres estudiante y tu universidad tiene alguna
 afiliación con Microsoft, es posible que seas candidato
 a [azure for students](https://azure.microsoft.com/en-us/free/students/)
 en donde puedes obtener 100 dolares gratis al año que
 puedes usar para correr tu servidor por al menos 3 meses
 con el plan B2s, planenado rotar el servidor entre la
 cuenta de tus amigos puedes correrlo teóricamente para siempre.

## Configura servidor

### 1- Entra al servidor con ssh

Esto ya es otro tema mas extenso de lo que va con esta guía,
 pero a grandes rasgos en cualquier Sistema Operativo debes
 poder correr en la terminal:

    ssh usuario@1.1.1.1

Te recomiendo usar solamente claves de ssh y cambiar la
 configuración a no permitir el uso de contraseñas además
 de cambiar el puerto para evitar lo mas posible el ataque
 de bots. Te puedes guiar de [mi configuración](https://github.com/ksobrenat32/notes/blob/main/ssh/sshd_config).

### 2- Configura el firewall

Esto igual dependerá de tu proveedor si lo estas corriendo
 localmente te recomiendo investigar como hacer port
 forwarding desde tu router, si es desde un proveedor como
 azure verifica la configuración de red.

Los puertos que es necesario abrir son:

Bedrock | Java
--- | ---
19132/udp | 25565/udp
19132/tcp | 25565/tcp

Además no olvides el puerto que uses para ssh, usa el
 protocolo tcp.

### 3- Crea la estructura y prepara la configuración

En linux siempre es mejor correr los programas con el menor
 privilegio, en este caso lo mejor es crear un usuario exclusivo
 para correr el servidor. Esto se consigue con:

    sudo useradd -d /opt/minecraft/ -s /sbin/nologin -u 1635 minecraft

A partir de ahí creamos los directorios y hacemos dueño al
 usuario que acabamos de crear.

    sudo mkdir /opt/minecraft
    chown -R minecraft: /opt/minecraft

### 4- Instalar lo necesario

El servidor ocupa una forma de correr java además de
 una forma de descargar archivos de red, se puede instalar
 con:

    sudo apt install openjdk-17-jdk wget build-essential git

### 5- Descargar lo necesario e instalar el servidor

En primer lugar debemos de entrar como el usuario que creamos.

    sudo -u minecraft bash
    cd
    pwd

Si todo esta bien, la respuesta al comando pwd será
 /opt/minecraft.

Ahora crearemos varios directorios para mejor organización

    mkdir downloads server

Empezaremos a descargar lo necesario

    cd downloads

Debes de bajar las ultimas versiones de:

- [paper](https://papermc.io/downloads) : La ultima versión
- [geyser](https://ci.opencollab.dev//job/GeyserMC/job/Geyser/job/master/) :
 La versión "Geyser-Spigot.jar"
- [floodgate](https://ci.opencollab.dev/job/GeyserMC/job/Floodgate/job/master/) :
 La versión "floodgate-spigot.jar"

Para bajarlo directamente en el servidor da click izquierdo
 y copia la url. Ya teniéndolo lo puedes descargar con wget,
 ejemplo:

    wget https://papermc.io/api/v2/projects/paper/versions/1.18.1/builds/81/downloads/paper-1.18.1-81.jar

Regresa a /opt/minecraft/server y crea los links, recuerda
 haber descargado la última versión.

    cd /opt/minecraft/server
    ln -s ../downloads/paper-1.18.1-81.jar paper-server.jar

Y ya podemos correr el servidor por primera vez con:

    java -jar paper-jar

Hubo un error, claro, es por que no has aceptado los
 términos y condiciones, debes entrar al archivo
 eula.txt y cambiar de false a true, se puede hacer
 con el editor de texto nano.

    nano eula.txt

Ahora si, vuelve a correr el comando anterior. Puedes
 probar si puedes ingresar pero de momento no podran
 ingresar usuarios de bedrock, para esto debemos configurar
 los links a geyser y floodgate. En primer lugar para
 detener el servidor se hace oprimiendo ctrl+c.

    cd /opt/minecraft/server/plugins
    ln -s ../../downloads/Geyser-Spigot.jar .
    ln -s ../../downloads/floodgate-Spigot.jar .

Se va a tener que modificar la configuración de geyser
 para que use floodgate, en el archivo /opt/minecraft/
server/server/plugins/Geyser-Spigot/config.yml la linea
 que debes cambiar es.

    auth-type: floodgate

Vuelve a correr el servidor

    cd /opt/minecraft/server
    java -jar paper-jar

Deberás ver como inicia tanto Geyser como floodgate inician.
 Como podrás ver no es practico tener que estarlo iniciando
 y perder el acceso a la terminal por lo que configuraremos
 el rcon, en primer lugar activalo y ponle una contraseña.
 Esto es desde el archivo server.propierties, las lineas:

    rcon.port=25575
    broadcast-rcon-to-ops=true
    enable-rcon=true
    rcon.password=UnaContraseñaSuperSegura

Ahora debemos descargar un cliente de rcon.

    cd /opt/minecraft
    git clone https://github.com/Tiiffi/mcrcon
    cd mcrcon
    make

Listo, abre otra conexión con ssh para correr el servidor y
 en la otra terminal:

    /opt/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p UnaContraseñaSuperSegura

Esto como ves, da acceso a la administración del servidor,
 bueno, ya que vemos que todo funciona es momento de automatizar,
 ya puedes salir del usuario minecraft.

### 6- Marcarlo como servicio

Para hacer que minecraft inicie junto al servidor, es necesario
 crear  un servicio, en este caso en el archivo
 /etc/systemd/system/minecraft-server.service y pegaremos lo
 siguiente

    [Unit]
    Description=Minecraft Server (Powered by paper)
    After=network.target

    [Service]
    User=minecraft
    ExecStart=/usr/bin/java -Xmx4096M -Xms512M -jar paper-server.jar nogui
    ExecStop=/opt/minecraft/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p UnaContraseñaSuperSegura stop
    WorkingDirectory=/opt/minecraft/server
    Nice=1
    KillMode=none
    SuccessExitStatus=0 1
    ProtectHome=true
    ProtectSystem=full
    PrivateDevices=true
    NoNewPrivileges=true

    [Install]
    WantedBy=multi-user.target

Cambiando tanto la contraseña como el valor de -Xmx al
 valor de RAM máxima que queremos asignar al servidor.

Iniciamos el servicio con

    sudo systemctl enable --now minecraft-server

Y listo, el servidor corre como servicio, puedes cerrar
 todas las terminales y veras que el servidor corre sin
 problemas. Para acceder la única forma es con rcon:

    /opt/mcrcon/mcrcon -H 127.0.0.1 -P 25575 -p UnaContraseñaSuperSegura

## Notas

- Es 100% recomendable idear una forma de respaldo, de
 preferencia diario.
- Es igualmente recomendable reiniciar el servidor al
 menos una vez al día.
- Recuerda actualizar constantemente el servidor, fallas
 como log4j surgen seguido.
- Hay mas formas de optimizar el server pero eso ya
 es muy personalizado y fuera de esta guía.
- Te recomiendo tener un firewall aparte del que te
 proporcione tu router o el servicio de nube que uses.

## Conclusión

Un servidor de minecraft es un proyecto muy divertido para
 convivir con amigos, solo recuerda que al ser un servicio
 expuesto a internet siempre es posible que lo comprometan
 ten mucho cuidado.
