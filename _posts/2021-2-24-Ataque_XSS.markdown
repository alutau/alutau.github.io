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



Vamos  a recrear un ataque xss a una pagina web para robar una cookie de sesión y loguearnos con una identidad robada.

Para ello crearemos dos contenedores docker con apache2 y php, uno será la víctima y otro el hacker.

En el docker de la víctima tenemos una página login.php en donde nos loguearemos con un usuario previamente creado, despues de loguearse el usuario navega por esa pagina web y acaba en la página hackeada para robarnos la sesión, en ese momento el hacker recibe tu cookie y podrá utulizarla para loguearse con tu sesión.

Login.php



![2021-02-24_16-35](/mis-assets/img/XSS/2021-02-24_16-35.png)



Página infectada

![2021-02-24_16-35_1](/mis-assets/img/XSS/2021-02-24_16-35_1.png)



