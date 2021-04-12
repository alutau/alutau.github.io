---
typora-copy-images-to: ../mis-assets/img/evitardos/
typora-root-url: ../
layout: post
title:  "Evitar ataques dos en apache"
date:   2021-04-12
categories: jekyll update
---

## Evitar Ataques dos en apache

Requisitos:

- Apache -> 2.4.10 (aunque es funcional para Apache 2.2)
- libapache2-mod-evasive -> 1.10.1-3
- Apache-utils -> 2.4.10-10+deb8u5 (para las pruebas con ApacheBench)

Lo primero que debemos hacer es instalar mod_evasive paa ello 

```
sudo apt install libapache2-mod-evasive
```



Después debemos editar el archivo de configuración */etc/apache2/mods-enabled/evasive.conf* con el siguiente contenido:

![2021-04-12_14-12](/../../Escritorio/capturas/puesta en marcha/2021-04-12_14-12.png)



Luego deberemos crear el directorio para que se guarden los logs ya que no lo hace automatico :

```
sudo mkdir -p /var/log/mod_evasive
```

y le asinaremos un dueño que será root:

```
sudo chown -R root:www-data /var/log/mod_evasive
```

y seguido reiniciamos el servidor apache

```
sudo systemctl restart apache2
```

Ahora vamos a probar el funcionamiento de mod_evasive con Apachebench:

![2021-04-12_14-15](/../../Escritorio/capturas/puesta en marcha/2021-04-12_14-15.png)

Podemos ver que de 100 solicitudes 70 fueron rechazadas.

Si revisamos el arcivo de logs 

![2021-04-12_15-00](/../../Escritorio/capturas/puesta en marcha/2021-04-12_15-00.png)

Si queremos montar este servicio en docker, la carpeta y el dockerfile quedarían así:![2021-04-12_14-34](/../../Escritorio/capturas/puesta en marcha/2021-04-12_14-34.png)



![2021-04-12_14-36](/../../Escritorio/capturas/puesta en marcha/2021-04-12_14-36.png)