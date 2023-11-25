FROM php:8.0-fpm

COPY ./sources.list /etc/apt/sources.list
RUN apt update && apt install -y zlib1g-dev libpng-dev libjpeg62-turbo-dev git unzip libgmp-dev

COPY ./docker-fpm.ini /usr/local/etc/php/conf.d/docker-fpm.ini
RUN docker-php-source extract \
  && docker-php-ext-install mysqli gd bcmath pdo_mysql pcntl gmp sockets\
  && docker-php-ext-configure gd --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd \
  && pecl install redis-5.3.4 \
  && docker-php-ext-enable redis opcache \
  && docker-php-source delete

WORKDIR /var/www/html

COPY ./composer-setup.php /var/www/html/composer-setup.php
RUN php composer-setup.php

ENV COMPOSER_ALLOW_SUPERUSER=1
RUN mv composer.phar /usr/local/bin/composer &&\
  composer config -g repos.packagist composer https://mirrors.aliyun.com/composer/
# RUN composer install

# RUN cp -R nexus/Install/install public/
# RUN chmod -R 0777 /var/www/html