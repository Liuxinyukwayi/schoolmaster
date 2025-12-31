FROM php:7.4-fpm

# 安装 mysqli 及常用扩展
RUN sed -i 's@deb.debian.org@mirrors.aliyun.com@g' /etc/apt/sources.list \
 && sed -i 's@security.debian.org@mirrors.aliyun.com@g' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
 && docker-php-ext-install mysqli pdo_mysql \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install gd \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html
