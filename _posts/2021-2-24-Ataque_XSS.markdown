---
typora-copy-images-to: ../mis-assets/img/XSS/
typora-root-url: ../
layout: post
title:  "Ataque XSS"
date:   2021-02-24
categories: jekyll update
---

##

### **ATAQUE XSS**



Vamos  a recrear un ataque xss a una página web para robar una cookie de sesión y loguearnos con una identidad robada.

Para ello crearemos dos contenedores docker con apache2 y php, uno será la víctima y otro el hacker.

En el docker de la víctima tenemos una página login.php en donde nos loguearemos con un usuario previamente creado, después de loguearse el usuario navega por esa pagina web y acaba en la página hackeada para robarnos la sesión, en ese momento el hacker recibe tu cookie y podrá utilizarla para loguearse con tu sesión.

Login.php

![2021-02-24_16-35](/mis-assets/img/XSS/2021-02-24_16-35.png)

![2021-02-24_15-25](/mis-assets/img/XSS/2021-02-24_15-25.png)



Página infectada

![2021-02-24_16-35_1](/mis-assets/img/XSS/2021-02-24_16-35_1.png)

Lógicamente si fuese real no nos avisaría de que ha sido hackeada.

![2021-02-24_15-25_1](/mis-assets/img/XSS/2021-02-24_15-25_1.png)

Además para que todo esto funcione hemos creado un documento dockerfile y una estructura como la primera  práctica de docker en cada una, es decir una para la victima y otra para el hacker



![2021-02-24_16-37_1](/mis-assets/img/XSS/2021-02-24_16-37_1.png)

Levantamos los dos servidores y vamos a recrear como ocurriría.

El hacker esta a la espera hasta que le llegue la cookie y mientras la victima esta en activo esperando usuarios.

Cuando un usuario visita la pagina de la victima login.php y se loguea correctamente.

![2021-02-24_15-25](/mis-assets/img/XSS/2021-02-24_15-25-1614343472245.png)

![2021-02-24_15-57](/mis-assets/img/XSS/2021-02-24_15-57.png)

Le asigna un número de sesión o cookie.

![2021-02-24_16-31](/mis-assets/img/XSS/2021-02-24_16-31.png)

Después visita la página hackeada.php y entonces es cuando el hacker intercepta la cookie de la victima mediante el fichero sesion_robada.php 

![2021-02-24_16-38](/mis-assets/img/XSS/2021-02-24_16-38.png)

Entonces el hacker usa la cookie en la página de la víctima y logra conectarse con la sesión de Mario.

![2021-02-24_16-33](/mis-assets/img/XSS/2021-02-24_16-33.png)