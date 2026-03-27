FROM nginx:stable-alpine

COPY .docker/nginx/conf.d/*.conf /etc/nginx/conf.d/

COPY --from=php /var/www/html/public /var/www/html/public
