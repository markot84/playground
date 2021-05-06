FROM php:8.0.3-apache

# Install Apache configuration
RUN a2enmod rewrite
COPY ./resources/site.conf /etc/apache2/sites-available/000-default.conf

# Install required packages
RUN apt-get update && apt-get install -y \
    unzip \
    libzip-dev \
    libpq-dev \
    cron \
    git

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add config file        
ADD resources/laravel.env /tmp/resources/.env

# Install PHP extensions
RUN docker-php-ext-install pdo_pgsql zip

# Install crontab
COPY ./resources/laravel-scheduler.conf /etc/
RUN crontab /etc/laravel-scheduler.conf

# Install entrypoint script
COPY ./resources/entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]