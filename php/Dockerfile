FROM php:7.1-fpm

MAINTAINER copyrenzhe <copyrenzhe@gmail.com>

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng12-dev \
        libfreetype6-dev \
        libssl-dev \
        libmcrypt-dev \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install mcrypt \
&& docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-configure gd \
      --enable-gd-native-ttf \
      --with-jpeg-dir=/usr/lib \
      --with-freetype-dir=/usr/include/freetype2 && \
      docker-php-ext-install gd \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli 


# swoole extension
RUN pecl install swoole \
    && docker-php-ext-enable swoole

# redis extension
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable   redis 

# memcached extension
RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
    && mkdir -p memcached \
    && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
    && ( \
        cd memcached \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r memcached \
    && rm /tmp/memcached.tar.gz \
    && docker-php-ext-enable memcached 


# Copy opcache configuration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini
COPY ./laravel.ini /usr/local/etc/php/conf.d
COPY ./laravel.pool.conf /usr/local/etc/php-fpm.d/

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
