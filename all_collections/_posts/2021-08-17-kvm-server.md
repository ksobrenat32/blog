---
layout: post
title: "Servidor de virtualización"
categories: kvm-qemu
visible: 1
author:
- ksobrenatural
---

# Introducción

Hola! En este post voy a explicar como tener un servidor de virtualización en Debian, aunque estas instrucciones no son exclusivas para debian, pues se puede conseguir lo mismo en cualquier instalación de GNU/Linux, solo que los comandos difieren un poco.

La utilidad de un servidor de virtualización es básicamente infinita, puedes emular cualquier Hardware ya sea para probar distintos sistemas operativos o para aislar distintos servicios, todo dependera de las posibilidades de tu hardware.

Además del servidor de virtualización, se configurará un puente de red, esto con el objetivo de que las maquinas virtuales puedan acceder a la red local.

# Requerimientos

- Computadora con Debian o derivada con Hardware de sobra para dar a maquinas virtuales
- Conexión de Ethernet directa. 
- Servidor SSH para conexión remota. (Es posible seguir la guía de forma local)

# Instalar lo necesario

En primer lugar es necesario instalar un conjunto de software para virtualizar. En este caso qemu-kvm

	apt install qemu qemu-kvm qemu-system qemu-utils libvirt-clients libvirt-daemon-system virtinst virt-manager bridge-utils

Y activamos el demonio de libvirt

	systemctl enable libvirtd

# Puente de red

Ahora es necesario verificar nuestra conexión a la red, esto se puede conseguir con el comando

	ip a

**Ejemplo del resultado** 

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
    		
Desde aquí podemos verificar que la interfaz de nuestra conexión por Ethernet tiene como nombre enp1s0 y la ip es 192.168.122.220, sabiendo esto podemos crear el puente, necesitamos entrar al archivo de configuración de interfaces. En Debian es /etc/network/interfaces

	vim /etc/network/interfaces

Dentro debemos de escribir la configuración del puente, tras modificar el archivo debe quedar así (revisa la ip, gateway y el nombre de la interfaz pues pueden no ser las mismas).

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

Ya que estamos seguros de nuestra configuración, reiniciaremos para que estas configuraciones se apliquen. Al reiniciar podremos ver nuestra configuración algo así.

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

Como podemos observar, ahora nuestra conexión se encuentra en la interfaz br0 que es nuestro puente.

**¡Listo!** Ya tenemos red, ahora podemos empezar a crear nuestras máquinas virtuales.

# Creación de máquinas virtuales

Para poder correr una máquina virtual, en primer lugar será necesario crear un disco para la máquina virtual. 

	# Creamos el directorio para nuestros discos
	mkdir kvm

	# Creamos el disco, es posible cambiar el nombre y el tamaño del disco
	qemu-img create -f qcow2 ./debian.qcow2 8G

	# Agregamos la configuración para conectarnos con qemu mientras usemos los comandos virt-install y virsh

	mkdir -p ~/.config/libvirt/
	echo 'uri_default = "qemu:///system"' | tee -a ~/.config/libvirt/libvirt.conf

	# (Opcional) Agregamos a nuestro usuario al grupo libvirt

	sudo usermod -a -G libvirt $(whoami)

Ya que tenemos el disco, podemos instalar el sistema operativo.

	virt-install \
   		--name debian `# Nombre de la máquina` \
    		--memory 1024 `# Cantidad de memoria RAM` \ 
    		--disk path=./debian.qcow2,size=8,format=qcow2,bus=virtio `# Ubicación del disco de la máquina virtual` \
		--virt-type kvm \
		--cpu host \
    		--vcpus 1 `# Número de núcleos para la maquina virtual` \
    		--os-type linux \
    		--os-variant debian11 `# Variante del sistema operativo` \
    		--network bridge=br0,model=virtio `# Nombre de la interfaz de red, en este caso br0` \
    		--graphics none `# Configuración sin interfaz gráfico` \
    		--console pty,target_type=serial \
    		--location 'http://ftp.debian.org/debian/dists/bullseye/main/installer-amd64/' \
    		`# centos stream 9 'http://mirror.stream.centos.org/9-stream/BaseOS/x86_64/os/'` \
    		--extra-args 'console=ttyS0,115200n8 serial'

Continuamos con nuestra instalación normal y ya podemos usar la máquina virtual. Puedes aprender como administrar la máquina virtual con el comando

	man virsh

# Conclusión

Crear un servidor de virtualización suele sonar más complicado de lo que es, espero que esta guía te haya sido de utilidad. :)

### Algunas fuentes

Last Dragon .(2021). Configurar Debian como servidor de virtualizacion con QEMU/KVM . Video de Youtube: https://www.youtube.com/watch?v=ADqWvmDbY0o
