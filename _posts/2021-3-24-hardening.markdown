---
typora-copy-images-to: ../mis-assets/img/hardening/
typora-root-url: ../
layout: post
title:  "Hardening: Seguridad en el servidor apache"
date:   2021-03-08
categories: jekyll update
---

##

Vamos a securizar un servidor  apache que correrá en un docker por esto modificaremos el archivo dockerfile para hacerlo más seguro.

En primer lugar vamos a evitar que se nos muestre el índice del contenido de nuestra página:

![Captura de pantalla 2021-03-24 130151](/mis-assets/img/hardening/Captura de pantalla 2021-03-24 130151.png)

Para evitarlo vamos a eliminar el modulo mod_autoindex.![Captura de pantalla 2021-03-24 1310292](/mis-assets/img/hardening/Captura de pantalla 2021-03-24 1310292.png)

Otra de las medidas que debemos tomar es que apache no devuelva que tipo de versión es, de esta manera no damos pistas para que no puedan explotar vulnerabilidades conocidas contra nuestro servidor.

Para ello modificamos el archivo de configuración de apache con el siguiente código:

![Captura de pantalla 2021-03-24 131352](/mis-assets/img/hardening/Captura de pantalla 2021-03-24 131352.png)

Y modificamos el dockerfile para que lo copie.

![Captura de pantalla 2021-03-24 131029](/mis-assets/img/hardening/Captura de pantalla 2021-03-24 131029.png)

Ahora ya no muestra que tipo de versión es tan solo dice que es apache.

![Captura de pantalla 2021-03-24 131530](/mis-assets/img/hardening/Captura de pantalla 2021-03-24 131530.png)