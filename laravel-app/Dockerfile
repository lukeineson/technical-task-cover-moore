# BUILD STAGE
# Base Image
FROM composer as builder

# Set the working directory
WORKDIR /app

# Copy the Composer files
COPY composer.json ./

# Install dependencies
RUN composer install --no-scripts --no-autoloader --ignore-platform-reqs

# Copy the rest of the application code
COPY . . 

# Generate the autoloader
RUN composer dump-autoload --no-scripts --optimize


# FINAL STAGE
# Base Image
FROM php:8.1-apache

# Set the working directory
WORKDIR /var/www/html

# Install system dependencies
RUN apt-get update && \
    apt-get install -y \
        git \
        zip \
        unzip

# Install PHP extensions required by Laravel
RUN docker-php-ext-install pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy the built application code from the builder stage
COPY --from=builder /app .

# Install production dependencies
RUN composer install --no-dev --optimize-autoloader --ignore-platform-reqs

# Expose the Apache port
EXPOSE 8000

# Set the entrypoint command
CMD bash -c "composer install && php artisan serve --host 0.0.0.0 --port=5001"



