FROM php:8.3.13-fpm-alpine

RUN apk update && apk add --no-cache \
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev \
    libzip-dev \
    git \
    unzip \
    icu-dev \
    openssl-dev \
    bash \
    --virtual .build-deps gcc g++ make



RUN docker-php-ext-install gd pdo pdo_mysql
    
RUN curl -sS http://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

