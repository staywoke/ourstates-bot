FROM laradock/php-fpm:7.0--1.2

MAINTAINER Peter Schmalfeldt <me@peterschmalfeldt.com>

RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN pecl install -o -f redis && rm -rf /tmp/pear && docker-php-ext-enable redis

COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

ADD ./laravel.ini /usr/local/etc/php/conf.d
ADD ./laravel.pool.conf /usr/local/etc/php-fpm.d/

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
