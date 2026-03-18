FROM php:8.5-fpm

# --- System dependencies ---
RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    nano less htop procps iputils-ping bash-completion \
    libpng-dev libjpeg-dev libwebp-dev libfreetype-dev libxpm-dev libtiff-dev \
    libonig-dev libxml2-dev libzip-dev \
    libmagickwand-dev pkg-config \
    libpq-dev \
    ffmpeg \
    jpegoptim optipng pngquant gifsicle webp \
    file \
    autoconf g++ make supervisor \
    wget gnupg2 ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# --- Node.js 24 + npm latest ---
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs \
    && npm install -g npm@latest \
    && rm -rf /var/lib/apt/lists/*

# --- PHP extensions ---
RUN pecl install redis imagick xdebug \
    && docker-php-ext-enable redis imagick xdebug

# --- Configure and install GD + standard extensions ---
RUN docker-php-ext-configure gd \
        --with-freetype \
        --with-jpeg \
        --with-webp \
        --with-xpm \
    && docker-php-ext-install -j$(nproc) \
        gd \
        pdo_mysql \
        pdo_pgsql \
        mysqli \
        zip \
        bcmath \
        pcntl \
        intl \
        exif

# --- Supervisor config folder ---
RUN mkdir -p /etc/supervisor/conf.d

# --- Composer ---
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# --- Workdir ---
WORKDIR /var/www/html

# --- Ports ---
EXPOSE 9000

# --- Start supervisor to run php-fpm, queue, vite ---
CMD ["supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]
