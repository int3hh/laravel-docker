FROM serversideup/php:8.4-fpm

USER root
RUN install-php-extensions gd exif
USER www-data

WORKDIR /var/www/html

COPY --chown=www-data:www-data composer.json composer.lock ./
RUN composer install --optimize-autoloader --no-dev --no-interaction --no-scripts --no-autoloader

COPY --chown=www-data:www-data . /var/www/html
RUN composer dump-autoload --optimize --no-dev
