```
mkdir docker-demo && cd docker-demo

docker container run \
  --rm \
  --name wordpress \
  --volume "$PWD/":/var/www/html \
  php:5.6-apache

vi index.php

<?php 
phpinfo();
?>


wget https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
tar -xvf wordpress-4.9.4-zh_CN.tar.gz

http://172.17.0.2/wordpress

docker container run \
  -d \
  --rm \
  --name wordpressdb \
  --env MYSQL_ROOT_PASSWORD=123456 \
  --env MYSQL_DATABASE=wordpress \
  mysql:5.7

docker container stop wordpress

FROM php:5.6-apache
RUN docker-php-ext-install mysqli
CMD apache2-foreground

docker build -t phpwithmysql .

docker container run \
  --rm \
  --name wordpress \
  --volume "$PWD/":/var/www/html \
  --link wordpressdb:mysql \
  phpwithmysql

docker container stop wordpress wordpressdb


```
