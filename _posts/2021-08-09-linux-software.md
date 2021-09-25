---
layout: post
title: "Usa GNU/Linux software"
categories: GNU-Linux
permalink: /Usa-GNU-Linux/3
visible: 0
author:
- ksobrenatural
---
En GNU/Linux la instalación y actualización del software se lleva a cabo un poco diferente, puede que estés acostumbrado a que para descargar un nuevo programa tengas que ir a una página web, descargar un ejecutable que te guiará por una serie de pasos hasta que logres instalarlo, si tienes suerte el programa buscará automáticamente por actualizaciones, pero si no, te quedaras con software desatendido.

## Entendamos a los administradores de paquetes

En linux se usan administradores de paquetes (programas) junto con sus repositorios. Imagínalo como una biblioteca, le pides al bibliotecario (administrador de paquetes) que te consiga un libro (paquete/programa), si está en la biblioteca te lo entregará, pero si no está te tocará conseguirlo externamente. 

La ventaja de tener una biblioteca(repositorio) es que sabes que los libros que están ahí están han sido revisados y autorizados por tu distribución de Linux, no contienen virus y probablemente sean de código abierto.

En Debian, y sus derivadas como kubuntu, el administrador de paquetes se llama aptitude, mejor conocido simplemente como apt, tienen un funcionamiento simple de comprender, en primer lugar tiene que investigar preguntándole a los repositorios, que como ya mencionamos son como grandes almacenes de software, si existe alguna versión nueva de los paquetes instalados o se ha añadido al catálogo algún programa nuevo, en caso de que así sea, te lo hará notar. Esto se consigue con el comando 

>> Se usa sudo pues la modificación de programas instalados solo lo puede realizar el administrador

    sudo apt update

En caso de que existan versiones nuevas del software instalado y se desee llevar a cabo la actualización, se puede llevar a cabo con

    sudo apt upgrade

¿Simple no?, si quieres buscar si existe un programa, por ejemplo, busquemos el programa neofetch, es un programa que te arroja datos básicos del equipo junto a una bonita imagen de la distribución linux, necesitamos ejecutar

    sudo apt search neofetch
        
        Sorting... Done
        Full Text Search... Done
        neofetch/focal 7.0.0-1 all
            Shows Linux System Information with Distribution Logo

Puedes ver que efectivamente lo encontró, su versión y una pequeña descripción de lo que hace el programa, para instalarlo se hace con

    sudo apt install neofetch

Se imprime mucha información, entre ella:
- Los paquetes que se van a instalar además del programa que deseas a estos se le llaman dependencias, muchas veces cuando alguien escribe un programa no quiere volver a escribir el código que alguien más ya escribió, por lo que mejor te hace descargar esos programas.
- Los paquetes sugeridos, programas que pueden mejorar tu experiencia
- Cuantos programas se actualizan, se remueven y se instalan
- El peso de la descarga y el espacio que ocupará en disco.
- Te pide tu autorización

Para seguir adelante, acepta si deseas instalarlo, y ahora prueba ejecutando

    neofetch

¿Bonito, no? bueno todo esto lo puedes también hacer desde un entorno gráfico, busca la tienda de aplicaciones en tu distribución, verás que es como la del teléfono, te simplifica las cosas pues desde ahí puedes instalar, actualizar y desinstalar programas, pero muchas veces hacerlo por terminal te facilita y acelera las cosas. 

## Alternativas

Algunas veces no vas a encontrar lo que buscas en tu administrador de paquetes, en este caso tienes varias opciones, buscar a la empresa que desarrolla el programa que quieres y ver si tiene instrucciones oficiales para linux, de ser así puede que lo tenga disponible en varias opciones, las principales son:

- **Ejecutable**: Simplemente lo corres ya sea desde la terminal o el administrador de archivos.
- **AppImage**: A grandes rasgos es lo mismo que un ejecutable.
- **Repositorio externo**: Algunos mantienen sus propios repositorios para poder ellos publicar su software sin restricciones, sin embargo, no lo recomiendo pues esto puede provocar fallos en tu sistema operativo a largo plazo por conflictos entre versiones de programas.
- **Flatpak o Snap**: A grandes rasgos es como un repositorio externo, pero aislado de tu sistema, es de las formas más fáciles que tienes de instalar software, te recomiendo mucho más flatpak pues es más rápido. La desventaja es que en primer lugar tendrás que instalar ya sea flatpak o snap y posteriormente instalar tu programa además de que tendrás que actualizar tanto tu repositorio base como flatpak/snap. La ventaja es su amplia cantidad de software, por ejemplo, la tienda oficial de flatpak es [flathub](https://flathub.org/home) puedes encontrar básicamente todo lo que necesites.
- **Instrucciones del desarrollador**: Algunas veces puedes encontrar instrucciones de como instalarlo, sin embargo recuerda tener cuidado con lo que tecleas en la terminal, en especial si usas sudo.

Todas las opciones tienen sus pros y contras, siempre primero busca el software en tu repositorio con *apt*, pero si no tienes otra opción, las que mayormente te recomiendo es buscar ejecutables, AppImage o la versión en flatpak, pero puedes probar lo que te sea más conveniente.

## ¡Ya casi!

Has entendido lo básico sobre la administración de programas con apt y algunas alternativas, recuerda siempre estar pendiente de lo que tecleas, nadie se hace responsable más que tú.

Ya casi terminas esta guía básica, pasa a la sección [Extra]({{site.baseurl}}/Usa-GNU-Linux/4)