FROM php:8.5-fpm

# system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    nano less htop procps iputils-ping bash-completion \
    libpng-dev libjpeg-dev libwebp-dev libfreetype6-dev \
    libonig-dev libxml2-dev libzip-dev \
    libmagickwand-dev \
    libpq-dev \
    ffmpeg \
    jpegoptim optipng pngquant gifsicle webp \
    file \
    autoconf g++ make \
    ca-certificates gnupg \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Node 24
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs \
    && npm install -g npm@latest

# PHP extensions
RUN pecl install redis imagick xdebug \
    && docker-php-ext-enable redis imagick xdebug

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp

RUN docker-php-ext-install -j$(nproc) \
        pdo_mysql \
        pdo_pgsql \
        mysqli \
        zip \
        bcmath \
        pcntl \
        intl \
        gd \
        opcache \
        exif

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
