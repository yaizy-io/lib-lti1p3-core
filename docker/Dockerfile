# Run from project root
# Docker build:
#   docker build --tag lib-ltp3-core -f ./docker/Dockerfile .
# Start
#   docker run -it -d --name lib-ltp3-core -v `pwd`:/var/www/html lib-ltp3-core
# Shell
#   docker exec -it lib-ltp3-core bash
# Tests
#   docker exec -it lib-ltp3-core ./vendor/bin/phpunit

FROM php:8.0-fpm
COPY --from=composer:2.3.7 /usr/bin/composer /usr/local/bin/composer

RUN apt-get update && apt-get install -y libzip-dev zip unzip libmcrypt-dev wget git cron libpq-dev libicu-dev  \
    && pecl install mcrypt-1.0.4 && docker-php-ext-enable mcrypt \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_pgsql pgsql \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache

RUN echo "short_open_tag = Off" >> /usr/local/etc/php/php.ini \
    && echo "upload_max_filesize = 50M" >> /usr/local/etc/php/php.ini \
    && echo "post_max_size = 50M" >> /usr/local/etc/php/php.ini \
    && echo "opcache.enable=1" >> /usr/local/etc/php/php.ini \
    && echo "session.use_cookies=0" >> /usr/local/etc/php/php.ini \
    && echo "opcache.memory_consumption=128" >> /usr/local/etc/php/php.ini \
    && echo "opcache.max_accelerated_files=3000" >> /usr/local/etc/php/php.ini \
    && echo "opcache_revalidate_freq = 100" >> /usr/local/etc/php/php.ini \
    && echo "error_log = /proc/self/fd/2" >> /usr/local/etc/php/php.ini \
    && echo "memory_limit = 512M" >> /usr/local/etc/php/php.ini \
    && echo "catch_workers_output = yes" >> /usr/local/etc/php/php.ini

ADD . /var/www/html
WORKDIR /var/www/html

RUN composer install