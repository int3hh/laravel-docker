############################################
# Stage 1: Build frontend assets
############################################
FROM oven/bun:alpine AS assets

WORKDIR /app
COPY package.json bun.lock vite.config.js ./
COPY resources ./resources
RUN bun install --frozen-lockfile && bun run build

############################################
# Stage 2: Production PHP image
############################################
FROM serversideup/php:8.4-fpm

USER root

# Install additional PHP extensions (pdo_pgsql, pcntl, redis, zip already included)
RUN install-php-extensions gd exif

USER www-data

# Copy application code
COPY --chown=www-data:www-data . /var/www/html

# Install composer dependencies
RUN composer install --optimize-autoloader --no-dev --no-interaction

# Copy built frontend assets from stage 1
COPY --from=assets --chown=www-data:www-data /app/public/build /var/www/html/public
