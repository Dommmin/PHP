FROM php:8.5-fpm

RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    nano less htop procps iputils-ping bash-completion \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    libmagickwand-dev \
    libwebp-dev \
    libheif-dev \
    libtiff-dev \
    pkg-config \
    ffmpeg \
    jpegoptim optipng pngquant gifsicle webp \
    file \
    autoconf g++ make \
    ca-certificates gnupg \
    wget \
    ripgrep \
    fd-find \
    bat \
    zoxide \
    && rm -rf /var/lib/apt/lists/*

# Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs

# PECL extensions
RUN pecl install redis imagick xdebug \
    && docker-php-ext-enable redis imagick xdebug

# PHP extensions
RUN docker-php-ext-install -j$(nproc) \
        pdo_mysql \
        pdo_pgsql \
        mysqli \
        zip \
        bcmath \
        pcntl \
        intl \
        exif \
        opcache

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
