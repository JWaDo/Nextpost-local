FROM php:7.2-apache

# Install PDO MySQL driver
# See https://github.com/docker-library/php/issues/62
RUN docker-php-ext-install pdo mysqli pdo_mysql

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y libpng-dev libfreetype6-dev libyaml-dev

RUN apt-get install git -y
RUN apt-get install -y curl

RUN pecl install yamL

RUN cd /var/www/html

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/usr/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

RUN docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-install gd\
    && docker-php-ext-install exif\
    && docker-php-ext-install bcmath

# Get mgp25 library
RUN composer require mgp25/instagram-php

# Workaround for write permission on write to MacOS X volumes
# See https://github.com/boot2docker/boot2docker/pull/534
RUN usermod -u 1000 www-data

# Enable Apache mod_rewrite
RUN a2enmod rewrite

# Enable yaml
#RUN a2enmod yaml
