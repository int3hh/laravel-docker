FROM php:8.4-fpm
# Install apt-transport-https for Yarn repository
RUN apt-get install apt-transport-https -y
RUN apt-get update && apt-get install -y gnupg2
# Add Yarn repository
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN curl -sL https://deb.nodesource.com/setup_20.x | bash -
# Install dependencies
RUN apt-get update && apt-get install -y \
    libpq-dev \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libonig-dev \
    libzip-dev \
    yarn \
    nodejs
# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
# Install extensions
RUN docker-php-ext-install pdo pdo_pgsql mbstring zip exif pcntl opcache
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install gd
# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN mkdir -p /var/www/html
WORKDIR /var/www/html
#echo filessizes
RUN echo "max_file_uploads=100" >> /usr/local/etc/php/conf.d/docker-php-ext-max_file_uploads.ini
RUN echo "post_max_size=120M" >> /usr/local/etc/php/conf.d/docker-php-ext-post_max_size.ini
RUN echo "upload_max_filesize=120M" >> /usr/local/etc/php/conf.d/docker-php-ext-upload_max_filesize.ini
# Configure opcache for production
RUN echo "opcache.enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.enable_cli=1" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.memory_consumption=256" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.interned_strings_buffer=16" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.max_accelerated_files=10000" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.validate_timestamps=0" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.save_comments=1" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
RUN echo "opcache.fast_shutdown=1" >> /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www
# Copy existing application directory contents
COPY . /var/www/html
# Copy existing application directory permissions
COPY --chown=www:www . /var/www/html
USER www
# Install composer dependencies
RUN composer install --optimize-autoloader --no-dev
# Laravel optimizations
RUN php artisan config:cache
RUN php artisan route:cache
RUN php artisan view:cache
RUN php artisan optimize

