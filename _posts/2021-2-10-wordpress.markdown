---
typora-copy-images-to: ../mis-assets/img/wordpress/
typora-root-url: ../
layout: post
title:  "Wordpress en docker"
date:   2021-02-10
categories: jekyll update
---

##

**WORDPRESS EN DOCKER**



Vamos a instalar wordpress en su propio contenedor.

1.Crea un direcorio de proyecto vacio en nuestro caso lo creamos  en docker_wordpress/ 

```
sudo mkdir docker_wordpress
```

2.Cambia al directorio recien creado

```
cd docker_wordpress/
```

3.Crea un archivo docker-compose.yml que inicie tu blog de Wordpress y una instancia de MySql separada con un montaje de volumen para la persistencia de datos:

```
sudo nano docker-compose.yml
```

![1](/mis-assets/img/wordpress/1.png)



Ahora, ejecuta docker-compose up -d en dicho directorio. Esto ejecuta docker-compose up en modo separado, extrae las imágenes de Docker necesarias e inicia los contenedores de wordpress y base de datos

```
sudo docker-compose up -d
```



En este punto, WordPress debería estar ejecutándose en el puerto 8000 de su Docker host http:localhost:8000, y puedes completar la “famosa instalación de cinco minutos” como administrador de WordPress.



![Captura de pantalla 2021-02-10 15:10:09](/mis-assets/img/wordpress/Captura de pantalla 2021-02-10 151009.png)



![Captura de pantalla 2021-02-10 15:12:28](/mis-assets/img/wordpress/Captura de pantalla 2021-02-10 151228.png)



Ya lo tenemos listo!!!!