FROM php:8.2-fpm

ENV COMPOSER_ALLOW_SUPERUSER=1

COPY ./sources.list /etc/apt/sources.list
COPY ./docker-fpm.ini /usr/local/etc/php/conf.d/docker-fpm.ini
COPY ./composer-setup.php /opt/composer-setup.php
RUN rm /etc/apt/sources.list.d/debian.sources \
  && apt update && apt install -y zlib1g-dev libpng-dev libjpeg62-turbo-dev git unzip libgmp-dev \
  && docker-php-source extract \
  && docker-php-ext-install mysqli gd bcmath pdo_mysql pcntl gmp sockets\
  && docker-php-ext-configure gd --with-jpeg \
  && docker-php-ext-install -j$(nproc) gd \
  && pecl install redis-5.3.4 \
  && docker-php-ext-enable redis opcache \
  && docker-php-source delete \
  && php /opt/composer-setup.php \
  && mv composer.phar /usr/local/bin/composer \
  && composer config -g repos.packagist composer https://mirrors.aliyun.com/composer/
