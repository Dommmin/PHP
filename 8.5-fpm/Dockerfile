FROM php:8.5-fpm

# Instalacja pakietów systemowych i rozszerzeń PECL w jednej warstwie
RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    nano less htop procps iputils-ping bash-completion \
    libonig-dev libxml2-dev libzip-dev libicu-dev libpq-dev \
    libmagickwand-dev libwebp-dev libheif-dev libtiff-dev \
    pkg-config ffmpeg \
    jpegoptim optipng pngquant gifsicle webp \
    file autoconf g++ make \
    ca-certificates gnupg wget \
    ripgrep fd-find bat zoxide \
    && curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get install -y nodejs \
    && pecl install redis imagick xdebug \
    && docker-php-ext-enable redis imagick xdebug \
    && docker-php-ext-install pdo_mysql pdo_pgsql zip bcmath pcntl intl exif \
    && rm -rf /var/lib/apt/lists/*

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
