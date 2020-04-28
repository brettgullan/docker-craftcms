# @see:
# https://github.com/prooph/docker-files/blob/master/php/7.4-fpm

ARG PHP_VERSION=""
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine

# Install required packages
RUN apk --update upgrade \
    && apk add --no-cache --virtual .build-deps \
    autoconf automake make gcc g++ libtool pkgconfig \
    # for intl extension
    icu-dev 

RUN apk add --no-cache --virtual .persistent-deps \
    zlib-dev libzip-dev \
    # for intl extension
    icu-libs

# Install base PHP modules
RUN docker-php-ext-install mysqli pdo_mysql zip

# Install PHP Intl module
RUN docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-install intl

# Install ImageMagick
RUN apk add --no-cache imagemagick-dev imagemagick \
    && printf "\n" | pecl install imagick \
    && docker-php-ext-enable imagick

# Install MySQL client binaries -- required for Craft CMS database backup
RUN apk add mysql-client

COPY ./conf.d/*.ini /usr/local/etc/php/conf.d/