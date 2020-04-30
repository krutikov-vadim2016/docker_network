FROM composer:latest AS composer
FROM ale10257/php7.0-fpm

COPY --from=composer /usr/bin/composer /usr/bin/composer

ARG USER_ID
ARG GROUP_ID

ADD ./php.ini /usr/local/etc/php/php.ini


RUN apt-get update && apt-get install -y --no-install-recommends apt-utils \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
	libmemcached-dev \
	libzip-dev \
    && docker-php-ext-install -j$(nproc) mbstring mysqli pdo_mysql zip opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# Куда же без composer'а.
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Добавим свой php.ini, можем в нем определять свои значения конфига
#ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini
ADD php-fpm.d/www.conf /usr/local/etc/php-fpm.d/www.conf


RUN usermod -u ${USER_ID} www-data && groupmod -g ${GROUP_ID} www-data

WORKDIR /var/www

USER "${USER_ID}:${GROUP_ID}"

CMD ["php-fpm"]
