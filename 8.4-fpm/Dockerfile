FROM php:8.4-fpm

ARG USER_ID=1000
ARG GROUP_ID=1000

RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    libpng-dev libjpeg-dev libwebp-dev libfreetype6-dev \
    libonig-dev libxml2-dev libzip-dev \
    imagemagick libmagick++-dev \
    libpq-dev \
    autoconf g++ make \
    supervisor \
    wget gnupg2 \
    && curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g npm@latest \
    && pecl install redis imagick xdebug swoole \
    && docker-php-ext-enable redis imagick xdebug swoole \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
        pdo_mysql pdo_pgsql mysqli \
        zip bcmath pcntl intl \
        gd opcache \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt-get install -y google-chrome-stable \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN groupmod -g ${GROUP_ID} www-data \
    && usermod -u ${USER_ID} -g www-data www-data \
    && mkdir -p /var/www /var/log/supervisor /var/run/supervisor \
    && chown -R www-data:www-data /var/www /var/log/supervisor /var/run/supervisor

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

EXPOSE 9000

CMD ["php-fpm"]
