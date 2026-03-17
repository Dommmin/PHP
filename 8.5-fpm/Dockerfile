FROM php:8.5-fpm

# system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git curl unzip zip \
    nano less htop procps iputils-ping bash-completion \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    libmagickwand-dev \
    libwebp-dev \
    libheif-dev \
    libtiff-dev \
    ffmpeg \
    jpegoptim optipng pngquant gifsicle webp \
    file \
    autoconf g++ make \
    ca-certificates gnupg \
    wget \
    ripgrep \
    fd-find \
    bat \
    zoxide \
    starship \
    && rm -rf /var/lib/apt/lists/*

# Node.js 24
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs

# PHP extensions via PECL
RUN pecl install redis imagick xdebug \
    && docker-php-ext-enable redis imagick xdebug

# PHP core extensions
RUN docker-php-ext-install -j$(nproc) \
        pdo_mysql \
        pdo_pgsql \
        mysqli \
        zip \
        bcmath \
        pcntl \
        intl \
        exif \
        opcache

# install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# verify ImageMagick formats (WebP, AVIF, HEIC, TIFF)
RUN php -r "print_r(Imagick::queryFormats());" | grep -E 'WEBP|AVIF|HEIC|TIFF' || exit 1

# set working directory
WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]