FROM php:8.5-fpm-alpine

# Runtime system packages
RUN apk add --no-cache \
    bash curl supervisor \
    freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev \
    icu-dev \
    libzip-dev oniguruma-dev \
    libpq-dev \
    jpegoptim optipng pngquant gifsicle

# PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j"$(nproc)" \
        bcmath exif gd intl mbstring pcntl \
        pdo_mysql pdo_pgsql pgsql sockets zip \
    && docker-php-ext-enable opcache

# Redis via PECL — build tools instalowane jako virtual group, usuwane po
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apk del .build-deps \
    && rm -rf /tmp/pear /var/cache/apk/*

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
