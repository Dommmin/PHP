FROM php:8.4-fpm

RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    libpng-dev libjpeg-dev libwebp-dev libfreetype6-dev \
    libonig-dev libxml2-dev libzip-dev \
    imagemagick libmagick++-dev \
    libpq-dev \
    autoconf g++ make \
    supervisor \
    rsync \
    wget gnupg2 ca-certificates \
    ffmpeg \
    jpegoptim optipng pngquant gifsicle \
    file \
    && curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g npm@latest \
    && pecl install redis imagick xdebug \
    && docker-php-ext-enable redis imagick xdebug \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
        pdo_mysql pdo_pgsql mysqli \
        zip bcmath pcntl intl \
        gd opcache exif \
    && mkdir -p /etc/apt/keyrings \
    && wget -q -O /etc/apt/keyrings/google-linux.gpg https://dl.google.com/linux/linux_signing_key.pub \
    && echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google-linux.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
        > /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt-get install -y google-chrome-stable \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]
