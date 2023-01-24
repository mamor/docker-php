FROM php:8.2-alpine

WORKDIR /app

RUN apk add --no-cache --virtual .build-deps autoconf g++ make && \
    pecl install swoole && \
    apk del .build-deps && \
    apk add --no-cache libstdc++ && \
    docker-php-ext-install pdo_mysql && \
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini && \
    echo 'extension=swoole.so' >> /usr/local/etc/php/conf.d/swoole.ini
