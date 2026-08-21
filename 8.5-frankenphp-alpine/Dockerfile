FROM dunglas/frankenphp:1-php8.5-alpine

# php-extension-installer — automatycznie dobiera pakiety dla danej wersji Alpine
COPY --from=mlocati/php-extension-installer:latest \
    /usr/bin/install-php-extensions /usr/local/bin/

RUN install-php-extensions \
    bcmath exif gd imagick intl mbstring opcache pcntl \
    pdo_mysql pdo_pgsql pgsql sockets zip redis \
    gmp soap xml simplexml

RUN apk add --no-cache \
    bash curl supervisor \
    jpegoptim optipng pngquant gifsicle \
    nodejs npm

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# --- Ports (FrankenPHP: HTTP/HTTPS/HTTP3, not php-fpm's 9000) ---
EXPOSE 80 443 443/udp

CMD ["frankenphp", "run", "--config", "/etc/frankenphp/Caddyfile"]
