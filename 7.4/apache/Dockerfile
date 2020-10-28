FROM php:7.4-apache

EXPOSE 8080

RUN apt-get update && \
    apt-get install -y --no-install-recommends git libmemcached-dev libzip-dev && \
    git clone https://github.com/php-memcached-dev/php-memcached.git /usr/src/php/ext/memcached && \
    git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis && \
    docker-php-ext-install bcmath memcached opcache pdo_mysql redis zip && \
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
    php -r "if (hash_file('sha384', 'composer-setup.php') === 'c31c1e292ad7be5f49291169c0ac8f683499edddcfd4e42232982d0fd193004208a58ff6f353fde0012d35fdd72bc394') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/bin/composer && \
    composer global require hirak/prestissimo
