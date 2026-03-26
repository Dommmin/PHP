FROM php:8.5-fpm-alpine

RUN apk add --no-cache \
    bash curl supervisor \
    freetype-dev libjpeg-turbo-dev libpng-dev libwebp-dev \
    icu-dev \
    libzip-dev oniguruma-dev \
    postgresql-dev \
    jpegoptim optipng pngquant gifsicle \
    autoconf g++ make \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j"$(nproc)" \
        bcmath exif gd intl mbstring opcache pcntl \
        pdo_mysql pdo_pgsql pgsql sockets zip \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apk del autoconf g++ make \
    && rm -rf /tmp/pear /var/cache/apk/*

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
