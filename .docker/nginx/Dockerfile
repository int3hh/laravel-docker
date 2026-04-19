FROM nginx:stable-alpine

COPY conf.d/*.conf /etc/nginx/conf.d/

RUN apk add --no-cache --virtual .ssl-build openssl \
 && mkdir -p /etc/nginx/ssl \
 && openssl req -x509 -nodes -days 365 \
      -subj "/C=RO/ST=RO/O=TEST-ORG, SRL/CN=TEST" \
      -addext "subjectAltName=DNS:test-docker-laravel.org" \
      -newkey rsa:2048 \
      -keyout /etc/nginx/ssl/nginx-selfsigned.key \
      -out /etc/nginx/ssl/nginx-selfsigned.crt \
 && apk del .ssl-build
