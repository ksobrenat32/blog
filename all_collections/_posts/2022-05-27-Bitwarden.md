---
layout: post
title: "Gestor de contraseñas"
categories: privacy
visible: 1
author:
- ksobrenatural
---

## Introducción

Internet es un lugar inseguro, y nosotros como humanos no somos muy listos,
 esto solo es una receta para el desastre y se demuestra con todas las veces
 que se filtran contraseñas, a simple vista pensamos que no nos afecta,
 hasta que nos damos cuenta que reutilizamos la misma contraseña desde hace
 10 años, tal vez con mínimas variaciones, un punto por aquí, un @ por allá
 y a la larga lo único que pasa es que terminamos olvidandolo y
 restableciendo la contraseña a la misma insegura de siempre, ¿acaso no hay
 una solución mejor?

De hecho si, para eso fueron creados los gestores de contraseñas, piensalo
 como una caja fuerte, ahí guardas contraseñas fuertes, difíciles,
 imposibles de aprender, y solo tienes que aprender una, la de esa caja
 fuerte, con esto podemos estar seguros de siempre tener una contraseña
 compleja y diferente para cada servicio, sin riesgo a olvidarla.

Justo eso es [bitwarden](https://bitwarden.com/), un gestor de contraseñas
 FOSS, lo que permite que cualquiera lo pueda utilizar, es increíblemente
 sencillo, versátil, seguro y lo mejor, gratuito.

## Requerimientos

- Cuenta de correo para el registro
- Contraseña fuerte: Esta contraseña es esencial, debe ser lo más compleja
 posible, esta nunca la debes haber usado antes y debe ser larga, puede que
 no muy compleja, pero si larga, recomendaría mínimo 30 caracteres que no
 es algo difícil, puedes juntar palabras sin relación pero que puedas
 recordar fácilmente, algo como "Lápiz pista morado pan casa perro agua
 tu-numero-de-la-suerte fecha importante @#$%#@" es muy seguro, pues al
 ser larga, es básicamente imposible que logren descifrarla aún con una súper
 computadora, solo recuerda evitar caracteres raros pues no todas las
 computadoras los soportan y te podrías encontrar en una situación en la que
 no puedas desbloquear tus contraseñas por que no tienes una 'ñ' o alguna
 cosa así, pues mejor evitarlo.

## Registro

En primer lugar accedemos a la página de bitwarden.

![Registro1]({{site.baseurl}}/assets/pictures/Bitwarden/Registro1.png)

Seleccionamos "Get Started" y tendremos que introducir nuestros datos.

![Registro2]({{site.baseurl}}/assets/pictures/Bitwarden/Registro2.png)

En mi caso estoy usando un correo temporal, debes recordar que sin tu
 contraseña maestra no podrás recuperar ninguna de tus demás contraseñas,
 por lo que anotala en algún lugar al menos en lo que la memorizas, puedes
 hacer uso de la pista, solo recuerda no ser muy explicito pues si la
 intentas recuperar te mostrará ese mensaje.

## Uso del gestor

Ya que tienes cuenta, puedes entrar por medio del link de [bitwarden](https://vault.bitwarden.com/)

![Uso1]({{site.baseurl}}/assets/pictures/Bitwarden/Uso1.png)

Debes de confirmar tu correo para tener acceso a todas las funciones,
 selecciona "Send Mail" y confirma el correo que te llegó.

![Uso2]({{site.baseurl}}/assets/pictures/Bitwarden/Uso2.png)

Una última configuración es el tamaño default del generador de
 contraseñas, a mi me gusta dejarlo así, de 32 caracteres y
 mezclando, letras, números y caracteres especiales

![Uso3]({{site.baseurl}}/assets/pictures/Bitwarden/Uso3.png)

Ahora desde el menú principal, podemos generar un Item, en este
 caso un acceso, dentro del acceso mismo metemos los datos del
 acceso y podemos generar una contraseña aleatoria nueva siguiendo
 la configuración previa.

![Uso4]({{site.baseurl}}/assets/pictures/Bitwarden/Uso4.png)

![Uso5]({{site.baseurl}}/assets/pictures/Bitwarden/Uso5.png)

Ahora solo tenemos que meternos al Acceso creado con nombre
 "google" y listo, podemos copiar tanto el usuario y la contraseña,
 no hay necesidad ni si quiera de saber la contraseña pues siempre
 podremos acceder a ella por medio de bitwarden.

## Conclusión

Con esta herramienta se pueden mitigar muchas fallas humanas
 relacionadas con contraseñas, lo único que quedaría es mejorar
 el sudo de la aplicación, esto se puede hacer instalando las
 aplicaciones de bitwarden en tu teléfono, computadora y para
 más comodidad en tu mismo navegador.

Puedes agregar seguridad con la autenticación de dos factores
 e incluso puedes correr tu propio servidor con el proyecto
 [vaultwarden](https://github.com/dani-garcia/vaultwarden).

Solo recuerda, tu contraseña maestra es lo más importante :)
