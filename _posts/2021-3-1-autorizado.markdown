---
typora-copy-images-to: ../mis-assets/img/autorizado/
typora-root-url: ../
layout: post
title:  "Ataque XSS"
date:   2021-03-01
categories: jekyll update
---

##

## Autenticación HTTP

> Según la [Wikipedia](https://es.wikipedia.org/wiki/Autenticación_de_acceso_básica) En el contexto de una transacción [HTTP](https://es.wikipedia.org/wiki/Hypertext_Transfer_Protocol), la **autenticación de acceso básica** es un método diseñado para permitir a un [navegador web](https://es.wikipedia.org/wiki/Navegador_web), u otro programa cliente, proveer credenciales en la forma de [usuario](https://es.wikipedia.org/wiki/Usuario_(informática)) y [contraseña](https://es.wikipedia.org/wiki/Contraseña) cuando se le solicita una [página](https://es.wikipedia.org/wiki/Página_web) al [servidor](https://es.wikipedia.org/wiki/Servidor_web).
>
> Ha sido diseñado con el fin de permitir a un navegador web o programa **aportar credenciales** basadas en nombre de usuario y [contraseña](https://es.wikipedia.org/wiki/Contraseña), que le permitan autenticarse ante un determinado servicio. El sistema   es muy sencillo de implementar, pero sin embargo no está pensado para   ser utilizado sobre líneas públicas, debido a que las credenciales que   se envían desde el cliente al servidor, aunque no se envían directamente en texto plano, se envían únicamente codificadas en [Base64](https://es.wikipedia.org/wiki/Base64), lo que hace que se puedan obtener fácilmente debido a que es   perfectamente reversible, es decir, una vez que se posee el texto   codificado es posible obtener la cadena original sin ningún problema,   por lo que la información enviada no es cifrada ni segura.



Veamos un ejemplo de autenticación básica en PHP:

Creamos un login.php



![2021-03-01_15-17](/mis-assets/img/autorizado/2021-03-01_15-17.png)

con este contenido:

```
<?php

$valid_passwords = array ("mario" => "qwerty");
$valid_users = array_keys($valid_passwords);

$user = $_SERVER['PHP_AUTH_USER'];
$pass = $_SERVER['PHP_AUTH_PW'];

$validated = (in_array($user, $valid_users)) && ($pass == $valid_passwords[$user]);

if (!$validated) {
  header('WWW-Authenticate: Basic realm="My Realm"');
  header('HTTP/1.0 401 Unauthorized');
  die ("Not authorized");
}

// If it arrives here, it is a valid user.
echo "<p>Welcome $user.</p>";
echo "<p>Congratulation, you are into the system.</p>";

    

```

   Lanzamos la web 

![2021-03-01_15-18](/mis-assets/img/autorizado/2021-03-01_15-18.png)

 y a visitarla nos pide credenciales:

![2021-03-01_15-19](/mis-assets/img/autorizado/2021-03-01_15-19-1614612821512.png)



nos logueamos:

![2021-03-01_15-19_1](/mis-assets/img/autorizado/2021-03-01_15-19_1-1614612835820.png)

y si miramos nos ha dado un codigo base64:

![2021-03-01_15-20](/mis-assets/img/autorizado/2021-03-01_15-20.png)

si esto lo desciframos :

![2021-03-01_15-21](/mis-assets/img/autorizado/2021-03-01_15-21-1614613080190.png)

Ya tenemos el usuario y contraseña.