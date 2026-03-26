FROM php:8.5-fpm-alpine

# php-extension-installer — automatycznie dobiera pakiety dla danej wersji Alpine
COPY --from=mlocati/php-extension-installer:latest \
    /usr/bin/install-php-extensions /usr/local/bin/

RUN install-php-extensions \
    bcmath exif gd intl mbstring opcache pcntl \
    pdo_mysql pdo_pgsql pgsql sockets zip redis

RUN apk add --no-cache \
    bash curl supervisor \
    jpegoptim optipng pngquant gifsicle

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
