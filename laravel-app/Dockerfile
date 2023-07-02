# Base image
FROM php:8.1-apache

# Set the working directory
WORKDIR /var/www/html

# Copy the application code
COPY . /var/www/html

# Set the ownership and permissions
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html/storage

# Install PHP extensions and dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libonig-dev \
        libzip-dev \
        zip \
        unzip \
    && docker-php-ext-install pdo_mysql zip \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install application dependencies
RUN composer install --no-scripts --no-autoloader --ignore-platform-reqs

# Generate the autoloader
RUN composer dump-autoload --optimize

# Enable Apache modules
RUN a2enmod rewrite

# Copy the Apache configuration file
COPY apache-config.conf /etc/apache2/sites-available/000-default.conf

# Expose the Apache port
EXPOSE 80

# Start the Apache server
CMD ["apache2-foreground"]


