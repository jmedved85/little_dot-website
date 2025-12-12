FROM php:8.4-fpm-alpine

# Useful PHP extension installer image, copy binary into your container
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install dependencies
RUN apk add --no-cache \
        bash \
        fcgi \
        git \
        libpng \
        libjpeg-turbo \
        freetype

# Install PHP extensions
RUN set -eux; \
    install-php-extensions \
        intl \
        zip \
        opcache \
        apcu \
        pdo \
        pdo_mysql \
        sqlite3 \
        dom \
        mbstring \
        gd \
        exif \
        curl \
        fileinfo \
        session

# Copy custom php.ini
COPY ./docker/php/php.ini /usr/local/etc/php/php.ini

# Install Xdebug and copy configuration
RUN set -eux; \
	install-php-extensions xdebug
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Set permissions for the uploads directory
RUN mkdir -p /var/www/html/public/uploads/documents && \
    chown -R www-data:www-data /var/www/html/public/uploads/documents && \
    chmod -R 777 /var/www/html/public/uploads/documents

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set composer to use system PHP
ENV COMPOSER_ALLOW_SUPERUSER=1

# Set working directory
WORKDIR /var/www/html