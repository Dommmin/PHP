FROM php:8.4-fpm

ARG USER_ID=1000
ARG GROUP_ID=1000

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    imagemagick \
    libmagick++-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g npm@latest

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    pdo_pgsql \
    mysqli \
    zip \
    bcmath \
    pcntl \
    intl \
    gd

RUN pecl install redis imagick \
    && docker-php-ext-enable redis imagick

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN groupmod -g ${GROUP_ID} www-data && \
    usermod -u ${USER_ID} -g www-data www-data && \
    chown -R www-data:www-data /var/www

WORKDIR /var/www
USER www-data

EXPOSE 9000