FROM php:8.4-fpm-alpine

ARG USER_ID=1000
ARG GROUP_ID=1000

RUN apk add --no-cache --virtual .build-deps \
    $PHPIZE_DEPS \
    git curl unzip \
    libpng-dev jpeg-dev libwebp-dev freetype-dev \
    oniguruma-dev libxml2-dev libzip-dev \
    imagemagick-dev \
    postgresql-dev \
    nodejs npm \
    && apk add --no-cache \
    libpng libjpeg libwebp freetype \
    oniguruma libxml2 libzip \
    imagemagick \
    postgresql-libs \
    supervisor \
    bash \
    && pecl install redis imagick \
    && docker-php-ext-enable redis imagick \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
       pdo_mysql pdo_pgsql mysqli \
       zip bcmath pcntl intl \
       gd opcache \
    && apk del .build-deps

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN addgroup -g ${GROUP_ID} www-data && \
    adduser -u ${USER_ID} -D -S -G www-data www-data && \
    mkdir -p /var/www && \
    chown -R www-data:www-data /var/www

RUN mkdir -p /var/log/supervisor /var/run/supervisor \
    && chown -R www-data:www-data /var/log/supervisor /var/run/supervisor

WORKDIR /var/www

USER www-data

EXPOSE 9000

CMD ["php-fpm"]