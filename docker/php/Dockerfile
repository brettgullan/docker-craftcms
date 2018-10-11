ARG PHP_VERSION=""
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine

RUN apk --update upgrade \
    && apk add zlib-dev

RUN docker-php-ext-install mysqli pdo_mysql zip