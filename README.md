# Tarea4SXE

>1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en esta guía sigue sus instrucciones para instalar LAMP en dicho contenedor.

Lo primero que debemos hacer es descargar la imágen de ubuntu con el tag indicado
-`sudo docker pull ubuntu:22.04` y con -`sudo docker images` comprobamos que existe

Ahora instalaremos LAMP, primero creamos y arrancamos el  contenedor:
-`sudo docker container create -i -t  -p  8080:80 --name pruebaWordpres ubuntu:22.04`
-`sudo docker container start --attach -i  pruebaWordpres`

Siguiendo los pasos de la guía,actualizamos los paquetes:
-`sudo apt update`

Instalamos apache2 y luego el servidor de base de datos mariaDB:
-`sudo apt install -y apache2 apache2-utils`
-`sudo apt install -y mariadb-server mariadb-client`


Utilizamos el siguiente comando para proteger la instalación mySQL
-`sudo mysql_secure_installation`

Instalamos php
-`sudo apt install -y php php-mysql libapache2-mod-php`

Tras eso nos preguntara nuestra región y zona horaria, despues comenzará a mostrarse una pantalla con el proceso de instalación

![Texto alternativo](fotoInstalacionphp.jpg)

Ahora probámos apache con el siguiente comando:
-`echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php`

En el navegador introducimos http://10.0.9.152 (la ip de mi máquina) y entraríamos a la página por defecto de apache

![Texto alternativo](probamosApacheServer.jpg)

Ahóra procedémos con la instalación de curl:
-`apt-get update && apt-get install -y curl`

y comprobamos el funcionamiento del archivo info.php
-`curl localhost/info.php`

![Texto alternativo](instalarCurl.jpg)

>2. Utiliza esta guía para instalar wordpress en el contenedor.

Ejecutamos el siguiente comándo para instalar los paquetes necesarios:
-`sudo apt install ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip
`
Creamos la instalación para el directorio:
-`sudo mkdir -p /srv/www`

-`sudo chown www-data: /srv/www`

-`curl https://wordpress.org/latest.tar.gz | tar zx -C /srv/www`

Usa echo para crear el archivo /etc/apache2/sites-available/wordpress.conf
y redirigir la configuración a este archivo
-`cat <<EOF > /etc/apache2/sites-available/wordpress.conf`

```
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
EOF
```
Reiniciamos apache2
-`service apache2 reload`

Una vez cargado apache2 de nuevo, ejecutamos los siguientes comandos
-`a2ensite wordpress`
-`a2ensite wordpress`

Reiniciamos apache2
-`service apache2 reload`

Ejecutamos el siguiente comando:
-`sudo a2dissite 000-default`

Y por ultimo,reiniciamos apache2
-`service apache2 reload`

Comandos a ejecutar para la configuracion de la base de datos para wordpress

1:-`mysql -u root`
2:-`CREATE DATABASE wordpress;`
3:-`CREATE USER 'wordpress'@'localhost' IDENTIFIED BY '<your-password>';`
4:-`GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER ON wordpress.* TO 'wordpress'@'localhost';`
5:-`FLUSH PRIVILEGES;`
6:-`QUIT;`

Comprobamos que accedemos a wordpress
http://(IP de la maquina):8080/wp-admin/setup-config.php

Y habrémos terminado la instalación y configuración y accedido a wordpress
