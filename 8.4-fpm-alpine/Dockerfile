FROM php:8.4-fpm-alpine

RUN apk add --no-cache \
    git curl unzip \
    libpng-dev jpeg-dev libwebp-dev freetype-dev \
    oniguruma-dev libxml2-dev libzip-dev \
    imagemagick-dev \
    postgresql-dev \
    nodejs npm \
    $PHPIZE_DEPS

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN pecl install redis imagick \
    && docker-php-ext-enable redis imagick \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
       pdo_mysql pdo_pgsql mysqli \
       zip bcmath pcntl intl \
       gd opcache

RUN composer install --no-dev --no-interaction --prefer-dist --optimize-autoloader \
    && npm ci \
    && npm run build \
    && rm -rf node_modules \
    && composer dump-autoload -o

RUN apk add --no-cache \
    libpng libjpeg libwebp freetype \
    oniguruma libxml2 libzip \
    imagemagick \
    postgresql-libs \
    supervisor \
    bash

WORKDIR /var/www

USER www-data

EXPOSE 9000

CMD ["php-fpm"]