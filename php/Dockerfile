# adicionar configuraçoes que não vem na imagem oficial

FROM php:8.3.13-fpm-alpine

# Atualiza os repositórios do Alpine e instala as bibliotecas necessárias
RUN apk update && apk add --no-cache \ 
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev \
    libzip-dev \
    git \
    unzip \
    icu-dev \
    libxml2-dev \
    openssl-dev \
    bash \
    --virtual .build-deps gcc g++ make


# Instala as extensões do PHP necessárias
RUN docker-php-ext-install gd pdo pdo_mysql

# pega o composer da web e instala na imagem 
RUN curl -sS http://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

