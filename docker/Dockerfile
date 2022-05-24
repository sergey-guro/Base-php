FROM php:8.0-fpm-alpine

ENV NPROC=4

# Installing PHP extensions
RUN apk update \
    && apk add --no-cache unzip \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache --virtual .php-deps \
        icu-dev \
        libzip-dev \
        libxml2-dev \
        openssl-dev \
        oniguruma-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        jpeg-dev \
        freetype-dev \
        libmcrypt-dev \
        libxml2-dev \
        git \
        postgresql-dev \
    && apk add --no-cache \
        icu-libs \
        libzip \
        libxml2 \
        openssl \
        oniguruma \
        libjpeg-turbo \
        libpng \
        freetype \
        libmcrypt \
        libcurl \
        libxml2 \
        libpq \
        postgresql-client \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure intl \
    && docker-php-ext-install -j${NPROC} \
        gd \
        zip \
        exif \
        opcache \
        bcmath \
        intl \
        pdo_pgsql \
        pcntl \
        soap \
    && pecl install redis xdebug-3.0.4 \
    && docker-php-ext-enable redis xdebug \
    # Remove unnecessary dependencies
    && apk del --no-network .phpize-deps \
    && apk del --no-network .php-deps \
    # Installing composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && rm /var/cache/apk/* \
    # Check PHP modules working
    && php --ri mbstring \
    && php --ri zip \
    && php --ri bcmath \
    && php --ri intl \
    && php --ri pdo_pgsql \
    && php --ri gd \
    && php --ri zip \
    && php --ri exif \
    && php --ri soap \
    && php --ri pcntl \
    # Install nginx
    && apk add --no-cache nginx

# Set-up php config
COPY docker/php/php-xdebug.ini /usr/local/etc/php/php.ini

# Set-up nginx config
COPY docker/nginx/default.conf /etc/nginx/conf.d/
COPY docker/nginx/nginx.conf /etc/nginx/nginx.conf

# Test nginx works
RUN nginx -t \
    # Forward nginx request and error logs to docker log collector
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

# Changing workdir
WORKDIR /var/www/app
