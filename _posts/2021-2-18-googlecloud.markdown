---
typora-copy-images-to: ../mis-assets/img/googlecloud/
typora-root-url: ../
layout: post
title:  "web en google cloud"
date:   2021-02-18
categories: jekyll
---

##

## **WEB EN GOOGLE CLOUD CON NODEJS**



Hoy vamos a crear una web en google cloud escrita con nodejs, en principio google cloud nos permite gratuitamente crear el proyecto y mantendrá el hosting abierto durante un tiempo, hasta que consumamos los creditos gratis.

Para ello tenemos que crear un cuenta nueva en google cloud tan solo tenemos que rellenar los datos que nos pide.

![1](/mis-assets/img/googlecloud/1.png)

Si seguimos las instrucciones de la pagina oficial https://cloud.google.com/appengine/docs/standard/nodejs/building-app

En primer lugar necesitamos crear un nuevo proyecto en nuestra cuenta de cloud.

![3](/mis-assets/img/googlecloud/3.png)

Una vez creado el proyecto vamos a instalar nodejs

```
sudo apt-get update
sudo apt-get install nodejs
```

Tambien necesitamos un paquete llamado npm que nos permitirá descargar las dependencias necesarias para el proyecto.

```
sudo apt-get install npm
```

Vamos a crear una carpeta donde iniciar nuestro proyecto

```
mkdir my-nodejs-service
```

Lo siquiente es crear un archivo llamado package.json se crea automaticamente iniciando npm en nuestro directorio de proyecto

```
cd /my-nodejs-service
npm init
```

Ejecutamos el siguiente comando para agregar express como una dependencia:

```
npm install express
```

Confirmamos que Express aparezca en el campo dependencies de archivo pakage.json y le agregamos la siguiente linea:

```
"scripts": {
  "start": "node server.js"
}
```

Ahora necesitamos crear un archivo llamado server.js al cual le daremos los valores de puerto de escucha,constantes,mensajes get,apps,etc, en la misma carpeta y agregamos el siguiente código:

```
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from App Engine!');
});

// Listen to the App Engine-specified port, or 8080 otherwise
const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}...`);
});
```

Después de acabar esto ya podemos probar que todo funciona, de momento en local, con el siguiente comando:

```
npm start
```

Visitamos http://localhost:8080/ y veremos el resultado.

![14](/mis-assets/img/googlecloud/14.png)

Ahora para subirlo a cloud necesitamos añadir un archivo llamado app.yaml con el siguiente contenido:

```
# [START app_yaml]
runtime: nodejs14
# [END app_yaml]
```

En este punto tenemos una estructura como esta:

```
my-nodejs-service/
  app.yaml
  package.json
  server.js
```

Ahora necesitamos instalar SDK de Cloud lo que nos proporcionará una herramienta de comandos gcloud para poder configurar y subir nuestro proyecto a google cloud.

Con el siguiente comando bajamos lo necesario:

```
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-327.0.0-linux-x86_64.tar.gz
```

Este archivo lo descomprimimos en nuestra carpeta raiz 

```
cd 
tar xj ./google-cloud-sdk-327.0.0-linux-x86_64.tar.gz
```

y lo instalamos con:

```
./google-cloud-sdk/install.sh
```

Durante la instalación nos vincula con nuestra cuenta de cloud y con nuestro proyecto.

A partir de aqui ya podemos usar los comandos gcloud, asi que volvemos a la carpeta de nuestro proyecto y subir nuestra pagina usando el comando:

```
gcloud app deploy
```

Para ver dicho servicio debemos usar el comando:

```
gcloud app browse
```

Y nos abrirá automaticamente el navegador, esta vez si que esta en la app de cloud ya no está en local.

![17](/mis-assets/img/googlecloud/17.png)

Ahora vamos a crear un pequeño formulario para nuestra página, en nuestro directorio del proyecto vamos a crear otro llamado views para almacenar los archivos HTML.

```
cd my-nodejs-service/
mkdir views
```

Dentro de este nuevo directorio creamos un archivo llamado form.html con el siguiente codigo html.

```
cd views/
sudo nano form.html
```

```
<!DOCTYPE html>
<html>
  <head>
    <title>My App Engine App</title>
  </head>
  <body>
    <h2>Create a new post</h2>
    <form method="POST" action="/submit">
      <div>
        <input type="text" name="name" placeholder="Name">
      </div>
      <div>
        <textarea name="message" placeholder="Message"></textarea>
      </div>
      <div>
        <button type="submit">Submit</button>
      </div>
    </form>
  </body>
</html>
```

En este punto tenemos  la siguiente estructura:

```
my-nodejs-service/
  views/
    form.html
  app.yaml
  package.json
  server.js
```

Necesitamos modificar el archivo server.js para que el formulario funcione y pueda hacer post en la carpeta submit, para eso agregamos las siguientes lineas:

```
const path = require(`path`);
app.get('/submit', (req, res) => {
  res.sendFile(path.join(__dirname, '/views/form.html'));
});
```

Tambien necesitamos agregar una dependencia para leer los datos de la solicitud:

```
npm install body-parser
```

y agregamos la dependecia al inicio del archivo server.js:

```
const bodyParser = require('body-parser');
```

Agrega la siguiente línea para configurar tu app de Express a fin de usar `body-parser`:

```
app.use(bodyParser.urlencoded({ extended: true }));
```

Agrega un controlador `POST` a tu archivo `server.js` para leer los datos:

```
app.post('/submit', (req, res) => {
  console.log({
    name: req.body.name,
    message: req.body.message
  });
  res.send('Thanks for your message!');
});
```

Despues de todos estos cambios el archivo server.js debe quedar asi:

```
'use strict';

// [START app]
const express = require('express');
const bodyParser = require('body-parser');
const path = require(`path`);

const app = express();

// [START enable_parser]
app.use(bodyParser.urlencoded({ extended: true }));
// [END enable_parser]

app.get('/', (req, res) => {
  res.send('Hello from App Engine!');
});

// [START add_display_form]
app.get('/submit', (req, res) => {
  res.sendFile(path.join(__dirname, '/views/form.html'));
});
// [END add_display_form]

// [START add_post_handler]
app.post('/submit', (req, res) => {
  console.log({
    name: req.body.name,
    message: req.body.message
  });
  res.send('Thanks for your message!');
});
// [END add_post_handler]

// Listen to the App Engine-specified port, or 8080 otherwise
const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}...`);
});
// [END app]

module.exports = app;
```

Ahora vamos a implementar los cambios realizados a cloud:

```
gcloud app deploy
```

Y confirmamos que todo funciona 

```
gcloud app browse
```

![21](/mis-assets/img/googlecloud/21.png)

Y ya tendriamos el formulario funcionando en google cloud.