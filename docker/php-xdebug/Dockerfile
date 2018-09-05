FROM myonlinestore/php:7.2-dev

COPY pool.ini /usr/local/etc/php-fpm.d/www.conf
COPY php.ini  /usr/local/etc/php/conf.d/90-myonlinestore.ini

EXPOSE 9000

WORKDIR /srv/app
