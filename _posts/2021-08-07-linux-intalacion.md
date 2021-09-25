---
layout: post
title: "Instalación de una distribución GNU/Linux"
categories: GNU-Linux
permalink: /Usa-GNU-Linux/1
visible: 0
author:
- ksobrenatural
---
Como ya lo mencioné, una ventaja del software libre es que es gratis :) no es necesario invertir ni un solo peso para seguir esta guía, teniendo en mente que ya cuentas con el Hardware. Considero que la mejor forma de empezar es probar una distribución de GNU/Linux por tu propia cuenta, en caso de que ya tengas la instalación, puedes pasar al nivel [Básico]({{site.baseurl}}/Usa-GNU-Linux/2) sin problemas, esta guía estará basada en kubuntu, un ubuntu con kde, pero si tienes otra distribución, solo cambia el gestor de paquetes.

En caso de que no desees instalar nada puedes no llevar a cabo la instalación y simplemente arrancar el sistema desde la usb sin afectar tu instalación de windows. Es posible llegar a  dentro del escritorio sin afectar tu instalación de windows y continuar esta guía con el método de arranque de la usb para ver si el sistema te convence, solamente recuerda que lo que hagas se borrará al apagar la computadora. 

## Requerimientos

Para probar GNU/Linux necesitarás una computadora y una memoria USB (Sin información) de al menos 4gb. En esta guía instalaremos kubuntu, es una variante de Ubuntu (la distribución más popular entre principiantes) que contiene el entorno de escritorio KDE, esto facilitara que te acostumbres viniendo de windows, si tú después decides que quieres probar otra distribución, algunas sugerencias son [Fedora](https://getfedora.org/es/) que es fantastica para probar las tecnologías nuevas [Debian](https://www.debian.org/) la mejor distribución si valoras estabilidad sobre novedad o [Arch](https://archlinux.org/) si quieres el software más nuevo.

Bueno primero que nada necesitamos descargar dos cosas, [ventoy](https://ventoy.net/en/index.html) y la iso(imagen) de [kubuntu](https://kubuntu.org/getkubuntu/).

#### Ventoy

1. Dirígete a la página de [descargas](https://github.com/ventoy/Ventoy/releases) y busca el ventoy-windows.zip más reciente y descárgalo. 

![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-01.png)

2. Ve a la carpeta y con clic derecho descomprímelo y accede a la carpeta descomprimida. (si no puedes descomprimir, puedes usar el programa [7zip](https://7-zip.org/))

![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-02.jpg)

3. Ejecuta Ventoy2Disk.exe (no es necesario que te aparezca la extensión .exe) te pedirá permisos de administrador.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-03.jpg)

4. Verás a tu memoria en el apartado de Device, asegúrate que es la correcta de lo contrario podrías formatear el dispositivo incorrecto, da click en install.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-04.jpg)

5. Te indicará dos advertencias que tendrás que aceptar

![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-05.jpg)
![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-06.jpg)

6. ¡listo! tu memoria usb ahora es un dispositivo desde el que se puede arrancar una gran variedad de sistemas operativos.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/ventoy-07.jpg)

#### Kubuntu

Ahora toca descargar [kubuntu](https://kubuntu.org/getkubuntu/).

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-page.png)

**Descarga el más reciente para evitar problemas de compatibilidad**

En caso de que tengas una computadora con recursos bajos, puedes probar [xubuntu](https://xubuntu.org/) o si tienes curiosidad, probar con otras variaciones de ubuntu , las puedes ver [aquí](https://ubuntu.com/download/flavours) pero si al final te decides quedar con Linux, te recomiendo cambiar a sistemas como Debian o Fedora.

Sin importar el que escojas, descárgalo dentro de tu memoria formateada con ventoy y al terminar, apaga la computadora.

## Secure boot

Desafortunadamente muchos fabricantes bloquean las opciones de arranque de las computadoras, tendrás que buscar en internet como acceder al bios de tu fabricante en específico, algunas teclas comunes son:

    * ASRock: F2 or DEL
    * ASUS: F2 for all PCs, F2 or DEL
    * Acer: F2 or DEL
    * Dell: F2 or F12
    * ECS: DEL
    * Gigabyte / Aorus: F2 or DEL
    * HP: F10
    * Lenovo (Laptops): F2 or Fn + F2
    * Lenovo (Escritorio): F1
    * Lenovo (ThinkPads): Enter then F1.
    * MSI: DEL
    * Microsoft Surface Tablets: Manten presionado el boton de subir volumen
    * Origin PC: F2
    * Samsung: F2
    * Toshiba: F2
    * Zotac: DEL

Ya dentro del menú de bios buscarás en el apartado de boot/arranque o de security/seguridad alguna opción que menciona al secure boot o arranque seguro y lo tendrás que desactivar, esto es para evitar errores al correr desde la usb. 

![]({{site.baseurl}}/assets/pictures/GNU-Linux/secure-boot.jpg)

Ya que lo desactivaste solo guardas y apagas la computadora, en caso de que te metas a Windows, apagas desde el menú de inicio de sesión.

## Arranque desde la USB

Al arrancar tu computadora pulsarás un botón de selector de arranque, de nuevo, esto varía entre fabricantes aunque comúnmente esta tecla es F10, F11, F12 o Esc, intentas con estas o buscas la especifica de tu computadora y te dejará escoger desde que dispositivo arrancar, obviamente arrancaras desde la usb.

Dejarás que kubuntu arranque hasta que te presente un menú, en este te dará varias opciones, selecciona kubuntu.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-boot.png)

Puedes cambiar el idioma y pulsa probar kubuntu

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-try.png)

## Dentro del escritorio

Ahora estás dentro del sistema linux, explóralo, métete a internet (firefox es el navegador incluido) escribe documentos de texto, explora la configuración (a decir verdad debian es un poco feo por default) solo recuerda que al apagar la computadora todos los cambios que hagas en el sistema se borrarán (cosas que descargues de internet, documentos que escribas, etc.).

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-desktop1.png)

Debes de probar la mayor parte de tu hardware, cámara, micrófono, tarjeta wifi. etc. si todo funciona por si solo, ¡Felicidades! tu computadora corre linux perfectamente, en el caso contrario tendrás que investigar que es lo que no te funciona e intentar solucionarlo. 

## Instalación

Si ya estás seguro de querer instalar linux tienes dos opciones, la primera es hacer un "dual boot" que significa tener tanto windows como linux en una misma computadora con un pequeño menú al arrancar para escoger uno ,obviamente al compartir el mismo disco tendrás que reducir el espacio de la partición de windows, la segunda es simplemente borrar tu disco y usar solamente linux. Ambos casos se presentarán en esta guía, sin embargo es absolutamente recomendable que hagas un respaldo de toda la información que contenga la computadora antes de proceder desde este punto.

#### Preparación Dualboot

Para iniciar este proceso tenemos que arrancar con windows, con una cuenta con privilegios de administrador accederemos al programa llamado "administrador de particiones o de discos" 

![]({{site.baseurl}}/assets/pictures/GNU-Linux/win-part.png)

Una vez dentro verás en la parte de abajo el disco 0 con mínimo 3 particiones, la más grande se marcará como tu disco c y este será al que reduciremos en espacio. Da un click derecho en la partición y elige reducir volumen o shrink volume, debes de escoger cuanto vas a reducir el espacio, lo mínimo para un entorno gráfico son 10 GB, pero si le piensas dar un uso relativamente frecuente, un mínimo de 30 GB es lo que yo te recomiendo, recuerda que en la opción de cuanto espacio lo quieres reducir te lo pide en MB así que si son 30 GB tendrás que escribir 30720. 

![]({{site.baseurl}}/assets/pictures/GNU-Linux/win-part2.jpg)

Una vez con ese espacio libre, el disco se verá así 

![](https://www.linuxtechi.com/wp-content/uploads/2019/10/Unallocated-partition.jpg) 

Observa el espacio negro, ese es el espacio que quedará para linux, ya puedes apagar windows.

#### Instalación

Recuerda tener tu disco respaldado incluso si quieres hacer un dualboot pues se modificaran particiones.  La instalación de linux se divide en varias partes, pero en un inicio volverás a arrancar desde la usb aunque esta vez seleccionarás instalar kubuntu.

1.  Escoge tu distribución de teclado, normalmente será latinoamericano o español.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-keyboard.png)

2. Escoge las aplicaciones, te recomiendo una instalación mínima pues será más ligera, pero si deseas tener múltiple software desde la instalación, escoge la normal. Selecciona ambas opciones extra, la primera es para tener un sistema actualizado desde el inicio y la segunda para instalar drivers y formatos no libres, esto facilitará tu experiencia.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-minimal.png)

3. Selecciona tu disco, si tu computadora tiene múltiples discos, escoge donde lo deseas instalar. En caso de que hayas llevado a cabo los pasos para el Dualboot, verás la opción de instalar junto a Windows 10.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-disk.png)

Si ves todo correcto, das aceptar.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-disk2.png)

4. Escoge tu localización, esto es para la zona horaria.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-timezone.png)

5. Crea tu usuario, recuerda usar una contraseña fuerte.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-user.png)

6. Empieza la instalación y espera a que termine y reinicias.

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-end.png)

## ¡Felicidades!

![]({{site.baseurl}}/assets/pictures/GNU-Linux/kubuntu-login.png)

Con tu sistema listo toca llevar a cabo una capacitación básica de su uso, pasa con gusto a la sección [Básico]({{site.baseurl}}/Usa-GNU-Linux/2).