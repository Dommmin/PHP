# Stage 1: Build the API image
FROM php:8.2-fpm-alpine

# Set working directory
WORKDIR /usr/src

# Install necessary dependencies
RUN apk update && apk add --no-cache \
    autoconf \
    gcc \
    g++ \
    make \
    pcre-dev \
    zlib-dev \
    file \
    bash \
    linux-headers \
    git \
    curl \
    libpng-dev \
    oniguruma-dev \
    libxml2-dev \
    libzip-dev \
    icu-dev \
    libc6-compat \
    zip \
    unzip \
    supervisor \
    mysql-client \
    imagemagick-dev \
    libtool \
    nodejs \
    npm \
    brotli-dev

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip intl

# Install npm package chokidar
RUN npm install --save-dev chokidar

# Install PHP PECL extensions
RUN pecl install redis xdebug imagick swoole

# Enable imagick PHP extension
RUN docker-php-ext-enable imagick

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
