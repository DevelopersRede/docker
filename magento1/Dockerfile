FROM php:5.6-apache
LABEL maintainer="João Batista Neto <neto.joaobatista@gmail.com>"
LABEL version="1.0.0"
LABEL description="Imagem Docker para Magento 1.9 com módulo de pagamento da Rede"

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y git unzip \
                           libfreetype6-dev \
                           libjpeg62-turbo-dev \
                           libxml2-dev 

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
 && docker-php-ext-install pdo \
    pdo_mysql \
    gd \
    soap

RUN usermod -u 1001 www-data
RUN groupmod -g 1001 www-data

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/bin/composer && chmod +x /usr/bin/composer

USER www-data
RUN git clone https://github.com/OpenMage/magento-mirror.git /var/www/html
RUN git clone https://github.com/DevelopersRede/magento1.git /tmp/modulo
RUN cp -R /tmp/modulo/* /var/www/html

RUN composer install

USER root
