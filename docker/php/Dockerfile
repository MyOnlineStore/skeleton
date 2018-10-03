FROM myonlinestore/php:7.2

RUN apt-get update && \
    apt-get install -y wget && \
    wget https://raw.githubusercontent.com/composer/getcomposer.org/1b137f8bf6db3e79a38a5bc45324414a6b1f9df2/web/installer -O - -q | php -- --install-dir=/usr/local/bin --filename=composer

COPY pool.ini /usr/local/etc/php-fpm.d/www.conf
COPY php.ini  /usr/local/etc/php/conf.d/90-myonlinestore.ini

EXPOSE 9000

WORKDIR /srv/app
