FROM nginx:stable-alpine

RUN mkdir -p /var/www/html

RUN apk update \
    && apk add openssl;

RUN openssl req -x509 -nodes -days 365 -subj "/C=RO/ST=RO/O=TEST-ORG, SRL/CN=TEST" -addext "subjectAltName=DNS:test-docker-laravel.org" -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt;

