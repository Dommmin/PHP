FROM php:8.4-fpm-alpine

RUN apk add --no-cache \
    git bash curl libpng-dev libxml2-dev zip unzip \
    freetype-dev libjpeg-turbo-dev libwebp-dev zlib-dev libzip-dev oniguruma-dev \
    postgresql-dev supervisor autoconf g++ make \
    chromium chromium-chromedriver harfbuzz nss freetype ttf-freefont \
    nodejs npm apache2-utils

ENV CHROME_BIN=/usr/bin/chromium-browser \
    CHROME_PATH=/usr/lib/chromium/ \
    PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true \
    PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

RUN npm install -g puppeteer

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
 && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip \
 && pecl install redis swoole && docker-php-ext-enable redis swoole

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

USER appuser
