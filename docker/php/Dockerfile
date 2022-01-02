FROM php:8.0.2-fpm-alpine

# Устанавливаем необходимые либы
RUN apk --update --virtual build-deps add \
        autoconf \
        make \
        gcc \
        g++ \
        libtool \
        icu-dev \
        curl-dev \
        pcre-dev \
        libpng-dev \
        libxml2-dev
RUN apk add \
        curl \
        bash \
        bash-completion \
        icu \
        pcre \
        libintl \
        libltdl \
        libxml2 \
        postfix \
        libzip-dev \
        oniguruma-dev


RUN docker-php-ext-install \
        zip \
        curl \
        bcmath \
        exif \
        gd \
        iconv \
        intl \
        mbstring \
        pdo_mysql

# Устанавливаем расширения для PHP

RUN docker-php-ext-install opcache

RUN apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev && \
  docker-php-ext-configure gd \
    --with-freetype=/usr/include/ \
    --with-jpeg=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd && \
  apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

RUN docker-php-ext-configure bcmath
RUN apk del build-deps

WORKDIR /var/www/backend
VOLUME ["/var/log/php"]
CMD postfix start && php-fpm