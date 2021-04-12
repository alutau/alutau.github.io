---
typora-copy-images-to: ../mis-assets/img/hardening/
typora-root-url: ../
layout: post
title:  "Hardening: Seguridad en el servidor apache"
date:   2021-03-24
categories: jekyll update
---

## PRÁCTICA 1

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



Además, modificaremos el archivo  000-default.conf para obligar al navegador de nuestros clientes a usar https (web segura) y también añadiremos otra cabecera para evitar que nuestro servidor cargue contenido de otros sitios. Esto se configura según las necesidades de cada servidor ya que quizás necesites cargar por ejemplo video de otra fuente, en ese caso se puede modificar la cabecera para ello.

![Captura de pantalla (531)](/mis-assets/img/hardening/Captura de pantalla (531).png)



Para obligar a usar el protocolo https además necesitaremos instalar el certificado digital de sitio seguro, tal  como hicimos en https://alutau.github.io/jekyll/update/2021/01/27/Apachessl.html.

El dockerfile para estas modificaciones quedaría así:



![Captura de pantalla (533)](/mis-assets/img/hardening/Captura de pantalla (533).png)

## PRÁCTICA 2

Vamos a instalar y configurar un firewall de aplicación llamado ModSecurity.

Instalar

```
sudo apt-get install modsecurity-crs 
```

Para configurar basta con copiar las reglas que trae definidas por defecto al directorio de configuración

```
sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf

```

Restablecemos apache y ya estará funcionando.

```
sudo systemctl restart apache2
```

Para comprobar vamos a intentar simular un ataque XSS poniendo un script en la pagina post.php

![image-20210407181934455](/mis-assets/img/hardening/image-20210407181934455.png)



Nos salta un 403.

![image-20210407182026796](/mis-assets/img/hardening/image-20210407182026796.png)

El dockerfile quedaría así:

![image-20210407182147681](/mis-assets/img/hardening/image-20210407182147681.png)



## PRÁCTICA 3

Vamos a instalar unas reglas OWASP para mitigar varios ataques típicos a las paginas web para ello necesitamos instalar en apache la librería *libapache2-mod-security2* .

Para instalar y configurar esta librería vamos a seguir estos comandos:

```
git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
cd owasp-modsecurity-crs.git
sudo mv crs-setup.conf.example /etc/modsecurity/crs-setup.conf
sudo mv rules/ /etc/modsecurity
```

Una vez realizado esto deberíamos ver el archivo */etc/apache2/mods-enabled/security2.conf así:

OJO si no quitas la ultima línea (IncludeOptional) da un error al reiniciar apache

![Captura de pantalla (561)](/mis-assets/img/hardening/Captura de pantalla (561).png)

Ahora vamos a comprobar que funciona, modificamos el archivo */etc/apache2/sites-availables/000-default.conf* y añadimos

```
SecRuleEngine On
SecRule ARGS:testparam "@contains test" "id:1234,deny,status:403,msg:'Cazado por Ciberseguridad'"
```

Reiniciamos apache y comprobamos introduciendo en el navegador:

```
localhost/index.html?testparam=test
localhost/index.html?exec=/bin/bash
localhost/index.html?exec=/../../ 

```

Estos son ataques básicos a la web excepto el primero los otros dos son cazados y bloqueados por modsecurity.

![Captura de pantalla (557)](/mis-assets/img/hardening/Captura de pantalla (557).png)



![Captura de pantalla (558)](/mis-assets/img/hardening/Captura de pantalla (558).png)



![Captura de pantalla (559)](/mis-assets/img/hardening/Captura de pantalla (559).png)

Esto prueba que modsecurity esta funcionando y para asegurarnos mas podemos ejecutar el comando

```
sudo tail /var/log/apache2/error.log
```

Y veremos como los ataques han sido detectados y bloqueados por modsecurity.

![Captura de pantalla (560)](/mis-assets/img/hardening/Captura de pantalla (560).png)