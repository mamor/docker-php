FROM php:8.0-apache

EXPOSE 8080

RUN apt-get update && \
    apt-get install -y --no-install-recommends cron git libjpeg-dev libmemcached-dev libpng-dev libzip-dev supervisor && \
    git clone https://github.com/php-memcached-dev/php-memcached.git /usr/src/php/ext/memcached && \
    git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis && \
    docker-php-ext-configure gd --with-jpeg && \
    docker-php-ext-install bcmath gd memcached opcache pcntl pdo_mysql redis sockets zip && \
    apt-get purge -y git && \
    apt-get autoremove -y --purge && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /usr/src/php/ext/memcached && \
    rm -rf /usr/src/php/ext/redis && \
    a2enmod rewrite && \
    echo '<VirtualHost *:8080>' > /etc/apache2/sites-available/000-default.conf && \
    echo 'DocumentRoot /var/www/html/public' >> /etc/apache2/sites-available/000-default.conf && \
    echo 'ErrorLog /dev/stderr' >> /etc/apache2/sites-available/000-default.conf && \
    echo 'CustomLog /dev/stdout combined' >> /etc/apache2/sites-available/000-default.conf && \
    echo '</VirtualHost>' >> /etc/apache2/sites-available/000-default.conf && \
    echo 'Listen 8080' > /etc/apache2/ports.conf && \
    cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini && \
    sed -i 's/expose_php = On/expose_php = Off/' /usr/local/etc/php/php.ini && \
    sed -i 's/;opcache.validate_timestamps=1/opcache.validate_timestamps=0/' /usr/local/etc/php/php.ini && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/bin/composer