typora-copy-images-to: ../mis-assets/img/protected/
typora-root-url: ../
layout: post
title:  "Protected"
date:   2021-03-01
categories: jekyll update



# Proteger con contraseña un directorio mediante autenticación básica



En esta guía práctica, le mostraremos cómo configurar un directorio protegido con contraseña mediante la autenticación básica.

Lo primero que debemos hacer en este ejemplo es crear un directorio para proteger en la raíz de nuestro documento. Digamos que la raíz de nuestro documento es **/ var / www / html** . Crearemos un directorio llamado **protected** en la raíz del documento - **/ var / www / html / protected** .



![2021-03-01_15-25](/home/usuari2/Escritorio/capturas/2021-03-01_15-25.png)



Lo siguiente que debe hacer es crear un archivo de contraseña con los usuarios. Usaremos la utilidad htpasswd proporcionada en el paquete principal de Apache. El archivo de contraseña se puede almacenar en cualquier lugar de su disco duro. En nuestro ejemplo, crearemos nuestro archivo htpasswd en / etc / htpasswd .

Tenga en cuenta que la ubicación del archivo htpasswd puede estar en cualquier lugar que desee en su unidad local. Solo necesita especificar la ruta completa al archivo htpasswd con la directiva AuthUserFile . Elija lo que considere que es una ubicación sensata para sus archivos de contraseñas.



![2021-03-01_15-36](/home/usuari2/Escritorio/capturas/2021-03-01_15-36.png)

#### Esta es la receta que se debe utilizar para configurar un directorio protegido con contraseña en el contexto del directorio:

```
<Directory "/var/www/html/protected">
  AuthType Basic
  AuthName "Authentication Required"
  AuthUserFile "/etc/htpasswd/.htpasswd"
  Require valid-user

  Order allow,deny
  Allow from all
</Directory>
```

![2021-03-01_15-38](/home/usuari2/Escritorio/capturas/2021-03-01_15-38.png)

La siguiente es la receta a utilizar para configurar un directorio protegido con contraseña en el contexto de htaccess:

Primero crearemos un archivo .htaccess en nuestro directorio protegido, / var / www / html / protected y configuraremos el contenido del archivo para que sea:

```
AuthType Basic
AuthName "Authentication Required"
AuthUserFile "/etc/htpasswd/.htpasswd"
Require valid-user

```

Ahora necesitamos crear un bloque **<Directory>** en httpd.conf o en el archivo de configuración principal de apache de  su distribución o en el archivo de configuración de su host virtual para que Apache procese este archivo htaccess.

```
<Directory "/var/www/html/protected">
  AllowOverride AuthConfig
  # The Options below is an example. Use what you deem is necessary.
  Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
  Order allow,deny
  Allow from all
</Directory>
```

![2021-03-01_15-43](/home/usuari2/Escritorio/capturas/2021-03-01_15-43.png)



Ahora puede ir a [http: // localhost / protected](https://translate.google.com/website?sl=auto&tl=es&u=http://localhost/protected) y el navegador le solicitará que ingrese sus credenciales. Si ingresa las credenciales correctas, se le otorgará acceso a **protected** . Si no ingresa las credenciales correctas, se le pedirá continuamente que  ingrese las credenciales hasta que ingrese las credenciales correctas o  haga clic en el botón **Cancelar** .

![2021-03-01_15-57_1](/home/usuari2/Escritorio/capturas/2021-03-01_15-57_1.png)



![2021-03-01_15-58](/home/usuari2/Escritorio/capturas/2021-03-01_15-58.png)

sPara Apache httpd 2.4, se ha renovado el mecanismo de autorización. A continuación, se muestra una muestra de una configuración que utiliza la autenticación HTTP básica para todo [DocumentRoot](https://translate.google.com/website?sl=auto&tl=es&u=http://httpd.apache.org/docs/current/mod/core.html%23documentroot) y permite el acceso público y no restringido a un directorio específico:

Suponiendo un valor [DocumentRoot](https://translate.google.com/website?sl=auto&tl=es&u=http://httpd.apache.org/docs/current/mod/core.html%23documentroot) de "/ srv / httpd / htdocs",.

```
<Directory "/srv/httpd/htdocs">
    Options +FollowSymLinks +Multiviews +Indexes
    AllowOverride None
    AuthType basic
    AuthName "private"
    AuthUserFile /srv/httpd/.htpasswd
    Require valid-user
</Directory>

<Directory /srv/httpd/htdocs/public>
    Require all granted
</Directory>
```

