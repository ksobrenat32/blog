---
layout: post
title: "Servidor de lxc en Debian"
categories: container
visible: 1
author:
- ksobrenatural
---

## Introducción

Hace tiempo hice una guía para crear un servidor de virtualización,
 pero me dí cuenta que muchas veces no ocupamos una maquina virtual
 entera, su disco, ram y cpu asignado puede llegar a ser mas de lo
 que ocupamos. Además de que después de un tiempo, las imágenes de
 disco aumentan mucho de tamaño. Para estos casos existen los
 contenedores.

En esta guía explicaré como correr contenedores lxc en Debian 11,
 además de configurar el puente de red para que los contenedores
 puedan acceder a la red local.

## ¿Qué es un contenedor?

Los contenedores son simplemente procesos corriendo en el host, se
 puede decir que son una especie de
 [chroot](https://man7.org/linux/man-pages/man2/chroot.2.html) con
 esteroides.

Los contenedores mas conocidos son los que se manipulan con
 [docker](https://www.docker.com/) o [podman](https://podman.io/),
 sin embargo, estos se plantean para un solo proceso, comúnmente
 una aplicación web, que se ve como algo sin importancia pues siempre
 se puede borrar y crear otro rápidamente y sin mayor problema,
 pero que tal si queremos el sistema operativo entero, con todo y su
 init, pues para eso existe [lxc](https://en.wikipedia.org/wiki/LXC)
 que trata a los contenedores mas como máquinas virtuales, sin dejar
 de ser ligero pues sigue usando el mismo kernel que con la ayuda de
 la combinación de [cgroups](https://en.wikipedia.org/wiki/Cgroups)
 y [namespaces](https://en.wikipedia.org/wiki/Linux_namespaces)
 consigue aislar los procesos completamente además de poder correr
 rootless.

## Requerimientos

- Computadora con Debian o derivada.
- Conexión de Ethernet directa (Para el puente de red)

## Instalar lo necesario

En primer lugar es necesario instalar lo necesario para correr
 los contenedores y crear el puente.

    sudo apt-get install lxc libvirt0 bridge-utils uidmap

## Puente de red

Ahora es necesario verificar nuestra conexión a la red, esto se
 puede conseguir con el comando.

    ip a

### Ejemplo del resultado

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
         valid_lft forever preferred_lft forever
    2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 52:54:00:da:fd:b4 brd ff:ff:ff:ff:ff:ff
        inet 192.168.122.220/24 brd 192.168.122.255 scope global dynamic enp1s0
         valid_lft 3538sec preferred_lft 3538sec
        inet6 fe80::5054:ff:feda:fdb4/64 scope link 
         valid_lft forever preferred_lft forever

Desde aquí podemos verificar que la interfaz de nuestra conexión
 por Ethernet tiene como nombre enp1s0 y la ip es 192.168.122.220,
 sabiendo esto podemos crear el puente, necesitamos entrar al archivo
 de configuración de interfaces. En Debian es /etc/network/interfaces

    vim /etc/network/interfaces

Dentro debemos de escribir la configuración del puente, tras modificar
 el archivo debe quedar así (revisa la ip, gateway y el nombre de la
 interfaz pues pueden no ser las mismas).

    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # The primary network interface
    allow-hotplug enp1s0
    iface enp1s0 inet static

    auto br0
    iface br0 inet static
        address 192.168.122.220
        netmask 255.255.255.0
        gateway 192.168.122.1
        bridge_ports enp1s0
        up /usr/sbin/brctl stp br0 on

Ya que estamos seguros de nuestra configuración, reiniciaremos para
 que estas configuraciones se apliquen. Al reiniciar podremos ver
 nuestra configuración algo así.

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
         link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
         inet 127.0.0.1/8 scope host lo
          valid_lft forever preferred_lft forever
         inet6 ::1/128 scope host 
          valid_lft forever preferred_lft forever
    2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master br0 state UP group default qlen 1000
         link/ether 52:54:00:da:fd:b4 brd ff:ff:ff:ff:ff:ff
    3: br0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
         link/ether da:64:e7:16:20:c8 brd ff:ff:ff:ff:ff:ff
         inet 192.168.122.220/24 brd 192.168.122.255 scope global br0
          valid_lft forever preferred_lft forever
         inet6 fe80::d864:e7ff:fe16:20c8/64 scope link 
          valid_lft forever preferred_lft forever

Como podemos observar, ahora nuestra conexión se encuentra en la
 interfaz br0 que es nuestro puente.

**¡Listo!** Ya tenemos red, ahora podemos empezar a crear nuestros
 contenedores con lxc.

## Configuración de lxc

Para correr contenedores sin root (lo mejor), debemos de revisar
 algunas cosas, en primer lugar, asegurarnos que este habilitado
 la función en el kernel.

    sudo sysctl kernel.unprivileged_userns_clone

Si es igual a 1, es que esta habilitado. En segundo lugar debemos
 verificar que nuestro usuario tenga asignadas las uid y gid,
 esto se hace con.

    cat /etc/s*id|grep $USER

Debe de salir algo como

    usuario:100000:65536
    usuario:100000:65536

Si todo es correcto, ahora modificaremos la configuración de lxc
 en /etc/lxc/default.conf debe de estar así:

    # Configurar el mapeo de uid y gid 
    lxc.idmap = u 0 100000 65536
    lxc.idmap = g 0 100000 65536

    # Configuración de red
    # Tipo veth para referirse a un puente de red
    lxc.net.0.type = veth
    # La interfaz de puente que creamos antes
    lxc.net.0.link = br0
    # Activar la red
    lxc.net.0.flags = up

    # Capacidades de los contenedores
    # Configuración para apparmor, usar cgroups y namespaces.
    lxc.apparmor.profile = lxc-container-default-cgns
    # Capacidad de crear contenedores dentro del mismo contenedor.
    # Cambia a 1 si deseas activarlo.
    lxc.apparmor.allow_nesting = 0

Para la red se repite la declaración del archivo default.conf,
 además añádiendo un límite a la cantidad de conecciones del 
 usuario, el archivo /etc/lxc/lxc-usernet debe de verse así:

    usuario veth br0 10

Por último debemos de agregar una variable de ambiente, esto
 se puede hacer en el .bashrc del usuario, debemos agregar:

    export DOWNLOAD_KEYSERVER="hkp://keyserver.ubuntu.com"

¡Listo!

## ¿Cómo usar contenedores lxc?

Como se mencionó al inicio, hicimos la configuración para
 correr contenedores rootless por lo que estos comandos se
 deben correr como usuario.

### Crear un contenedor

Para usar un contenedor primero lo debemos de generar a partir
 de una plantilla, en este caso la plantilla "download" y se
 le asigna un nombre para identificarlo. La plantilla download
 es especial, te permitirá crear un contenedor a partir de
 varias distribuciones, escoge la que más te convenga.

    lxc-create --name test -t download

### Manipular el contenedor

Para iniciar el contenedor en modo no privilegiado:

    lxc-unpriv-start test

Para obtener un shell dentro:

    lxc-unpriv-attach test

Para detener el contenedor:

    lxc-stop test

## Dudas

### No tengo red

Al tener que conectarse a la red local, al contenedor le toma
 algo de tiempo recibir una ip, ten paciencia y probablemente
 se resuelva.

### Me marca un error en la llave gpg

Para eso es la variable que exportamos, sal y vuelve a entrar
 para que se cargue correctamente.

### Error en la configuración

Algunas veces se queja por no tener una configuración individual
 del usuario, configurala en $HOME/.config/lxc/default.conf ,
 debe llevar básicamente lo mismo que la configuración global.

    lxc.include = /etc/lxc/default.conf
    lxc.idmap = u 0 100000 65536
    lxc.idmap = g 0 100000 65536
 
    lxc.net.0.type = veth
    lxc.net.0.link = br0
    lxc.net.0.flags = up

    lxc.apparmor.profile = lxc-container-default-cgns
    lxc.apparmor.allow_nesting = 0

### ¿Dónde se almacena el contenedor?

Los contenedores se almacenan en $HOME/.local/share/lxc en caso
 de querer modificar su ubicación se puede hacer desde la
 configuración individual de cada contenedor, en mi caso prefiero
 hacer un soft link al directorio donde lo quiero.

## Conclusión

Las máquinas virtuales son muy útiles pero muchas veces son mas
 de lo que necesitamos, por lo que para la mayoría de casos un
 contenedor lxc es mucho mas útil. Solo recuerda que no es el
 mismo nivel de aislamiento. Espero que esta guía te haya sido
 de utilidad, hasta luego. :)
