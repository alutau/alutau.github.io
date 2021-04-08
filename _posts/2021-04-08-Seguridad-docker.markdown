---
typora-copy-images-to: ../mis-assets/img/seguridad_docker/
typora-root-url: ../
layout: post
title:  "Seguridad docker"
date:   2021-04-08
categories: jekyll update
---

# SEGURIDAD EN DOCKER

Para tratar de hacer docker más seguro vamos a tomar una serie de medidas básicas y buenas prácticas.

Empezaremos por crear un nuevo grupo llamado docker y así poder incluir permisos a los usuarios que creamos conveniente para ejecutar docker. De momento, vamos a añadir a nuestro usuario.

![Captura de pantalla (539)](/mis-assets/img/seguridad_docker/Captura de pantalla (539).png)

Como el demonio de docker se ejecuta como superusuario tenemos que evitar que los usuarios puedan modificar su configuración. El archivo de configuración esta en */etc/docker/daemon.json* y modificaremos los parámetros:

![2021-03-22_15-22](/mis-assets/img/seguridad_docker/2021-03-22_15-22.png)

###  Seguridad e contenedores 

Las medidas a tomar en los contenedores serán:

Asignar un ulimit que nos permitirá limitar el número de procesos máximos:

```
docker run --ulimit nofile=512:512 --rm debian sh -c "unlimit -n"
```

 También limitaremos el uso de la cpu con el parámetro :

![2021-03-22_15-38](/mis-assets/img/seguridad_docker/2021-03-22_15-38-1617876005511.png)

Por defecto el usuario del contenedor es root esto lo podemos forzar  con el flag *-u uuid*  por ejemplo si usamos el usuario 4400 nos dará error de permisos. Otro flag relacionado con permisos es *--privileged* 

![2021-03-22_15-41](/mis-assets/img/seguridad_docker/2021-03-22_15-41.png)

Si no le damos *--privileged* nos devuelve que no tiene permisos.

Cabe la posibilidad de montar nuestro socket docker dentro de otro contenedor usando :

![2021-03-22_15-43](/mis-assets/img/seguridad_docker/2021-03-22_15-43.png)

## Seguridad en imagenes

Es recomendable utilizar solo imágenes oficiales o bien valoradas en dockerhub y que tengamos acceso a su dockerfile para ver que es lo que ejecutan, de lo contrario podrían venir con malware de regalo.

## TRIVI

Trivi es un programa que realiza un test de vulnerabilidades a una imagen de docker y las clasifica de mayor a menor prioridad. Vamos a probarla con una imagen antigua de wordpress:

![Captura de pantalla (542)](/mis-assets/img/seguridad_docker/Captura de pantalla (542).png)

![Captura de pantalla (543)](/mis-assets/img/seguridad_docker/Captura de pantalla (543).png)

Y nos devuelve la mayoría de las vulnerabilidades en una prioridad alta o crítica.