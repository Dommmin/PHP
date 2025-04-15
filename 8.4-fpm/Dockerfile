FROM php:8.4-fpm

# Arguments for user permissions
ARG USER_ID=1000
ARG GROUP_ID=1000

# Install system dependencies
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
    nodejs \
    npm \
    imagemagick \
    libmagick++-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js 22 LTS
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g npm@latest

# Configure and install PHP extensions
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

# Install PECL extensions
RUN pecl install redis imagick \
    && docker-php-ext-enable redis imagick

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Set proper permissions
RUN usermod -u ${USER_ID} www-data \
    && groupmod -g ${GROUP_ID} www-data \
    && chown -R www-data:www-data /var/www

# Switch to non-root user
USER www-data

# Expose port 9000
EXPOSE 9000