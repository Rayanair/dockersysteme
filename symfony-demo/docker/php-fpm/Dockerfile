FROM php:8.2-fpm-alpine

WORKDIR /var/www/html

RUN apk --no-cache add \
    coreutils \
    freetype \
    libjpeg-turbo \
    libpng \
    freetype-dev \
    libjpeg-turbo-dev \
    libpng-dev


RUN docker-php-ext-configure gd --with-freetype --with-jpeg


RUN docker-php-ext-install -j$(nproc) \
    gd \
    pdo_mysql

COPY symfony.ini /usr/local/etc/php/conf.d/
COPY others.ini /usr/local/etc/php/conf.d/
COPY symfony.pool.conf /usr/local/etc/php-fpm.d/

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"
RUN apk --no-cache add icu-dev

RUN docker-php-ext-install intl

COPY . .

RUN ls -la

EXPOSE 9000
CMD ["php-fpm"]