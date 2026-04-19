FROM serversideup/php:8.4-fpm

ARG USER_ID=1000
ARG GROUP_ID=1000

USER root
RUN install-php-extensions gd exif \
 && docker-php-serversideup-set-id www-data ${USER_ID}:${GROUP_ID} \
 && docker-php-serversideup-set-file-permissions --owner ${USER_ID}:${GROUP_ID} --service fpm
USER www-data

WORKDIR /var/www/html

COPY --chown=${USER_ID}:${GROUP_ID} composer.json composer.lock ./
RUN composer install --optimize-autoloader --no-dev --no-interaction --no-scripts --no-autoloader

COPY --chown=${USER_ID}:${GROUP_ID} . /var/www/html
RUN composer dump-autoload --optimize --no-dev
