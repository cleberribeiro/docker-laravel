FROM php:7.3.6-fpm-alpine3.10

WORKDIR /var/www

RUN apk add --no-cache shadow bash mysql-client \
&& docker-php-ext-install pdo pdo_mysql \
&& usermod -u 1000 www-data \
&& rm -rf /var/www/html \
&& ln -s public html \
&& curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer 

COPY . /var/www
RUN chown -R www-data:www-data /var/www

USER www-data

RUN composer install && \
    cp .env.example .env && \
    php artisan key:generate && \
    php artisan config:cache && \
    php artisan migrate

EXPOSE 9000

ENTRYPOINT ["php-fpm"]