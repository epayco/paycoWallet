# PaycoWallet

Este es un desarrollo de prueba para PAYCO

## Notas iniciales:

Este proyecto está diseñado para correr en un ambiente LAMP con Symfony 4.0

### Prerrequisitos

Debe tener instalado:

```
Composer
MySql Server
Linux
PHP > 7.1.3
Apache server
```

### Instalación


Este es el proceso de instalación para correr la aplicacion server SOAP

Se recomienda trabajar con virtual hosts debido a que esta aplicación está desarrollada con symfony 4.X

Ejemplo de virtual host:

Se debe modificar el archivo /etc/hosts agregando nuestro server (dominio local por el cual queremos acceder)


``` 
127.0.0.1       localhost
127.0.1.1       Elnombredetuequipo

127.0.0.1       soap.payco.com #nuestro server


# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

```

Creamos este archivo ```
                     soap.payco.com.conf
                     ``` o con el nombre que queramos terminado en .conf y se crea en ```/etc/apache2/sites-aviable 
                    ```

```
<VirtualHost *:80>
    ServerName soap.payco.com  #nombre del servidor dado en /etc/hosts
    ServerAlias soap.payco.com #(recomendado el mismo ServerName)
    DocumentRoot /Ruta/Hasta/El/Proyecto/paycoWallet/soap/public
    <Directory /Ruta/Hasta/El/Proyecto/paycoWallet/soap/public>
        AllowOverride None
        Order Allow,Deny
        Allow from All

        <IfModule mod_rewrite.c>
            Options -MultiViews
            RewriteEngine On
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteRule ^(.*)$ index.php [QSA,L]
        </IfModule>
    </Directory>

    # uncomment the following lines if you install assets as symlinks
    # or run into problems when compiling LESS/Sass/CoffeeScript assets
    # <Directory /var/www/project>
    #     Options FollowSymlinks
    # </Directory>

    ErrorLog /var/log/apache2/project_error.log
    CustomLog /var/log/apache2/project_access.log combined

    # optionally set the value of the environment variables used in the application
    #SetEnv APP_ENV prod
    #SetEnv APP_SECRET <app-secret-id>
    #SetEnv DATABASE_URL "mysql://db_user:db_pass@host:3306/db_name"
</VirtualHost>

```


Luego se crea un enlace simbólico en /etc/apache2/sites-enabled

```
sudo ln -s /etc/apache2/sites-available/soap.payco.com.conf /etc/apache2/sites-enabled/soap.payco.com.conf

```

Por último habilitamos el módulo de reescritura de apache en caso de no tenerlo habilitado

```
sudo a2enmod rewrite
```

Luego reiniciamos el servicio de apache

```
sudo service apache2 restart
```

Paso 1

```
git clone https://github.com/ju4nr3v0l/paycoWallet.git
```


Paso 2

Modificamos nuestro virtual host en ```/etc/apache2/sites-aviable ``` y cambiamos las lineas 
```
 DocumentRoot /Ruta/Hasta/El/Proyecto/paycoWallet/soap/public
 <Directory /Ruta/Hasta/El/Proyecto/paycoWallet/soap/public>
```
 por la ruta donde acabamos de clonar nuestro repositorio

Paso 3

```
cd /paycoWallet/soap
```

Paso 4

```
composer install
```

Paso 5

```
composer update
```

Paso 6

En la raiz del proyecto modificar en el archivo .env la linea que se describe a continuación con sus datos de conexión a su servidor MySql y el nombre de la base de datos. 

```
DATABASE_URL=mysql://usuario:contraseña@127.0.0.1:3306/baseDeDatos
```

Además se debe modificar el bloque de swiftmailer asi: (copiar y pegar. Eliminar el bloque preexistente en el archivo)

```
###> symfony/swiftmailer-bundle ###
# For Gmail as a transport, use: "gmail://username:password@localhost"
# For a generic SMTP server, use: "smtp://localhost:25?encryption=&auth_mode="
# Delivery is disabled by default via "null://localhost"
MAILER_URL=gmail://paycowallet@gmail.com:paycowallet123@localhost
###< symfony/swiftmailer-bundle ###
```


Paso 7

```
php bin/console doctrine:database:create
```

Paso 8

```
php bin/console doctrine:schema:update --force
```





## Probando la aplicación

Esta es la doc de la API

```
https://documenter.getpostman.com/view/2689877/RWEiMJyM
```

Usando los endPoints con los ejemplos se puede iniciar el testeo de la app.


