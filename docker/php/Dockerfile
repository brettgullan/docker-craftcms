ARG PHP_VERSION=""
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine

# Install required packages
RUN apk --update upgrade \
    && apk add autoconf automake make gcc g++ libtool pkgconfig \
    && apk add zlib-dev libzip-dev

# Install base PHP modules
RUN docker-php-ext-install mysqli pdo_mysql zip

# Install ImageMagick
RUN apk add --no-cache imagemagick-dev imagemagick \
    && printf "\n" | pecl install imagick \
    && docker-php-ext-enable imagick