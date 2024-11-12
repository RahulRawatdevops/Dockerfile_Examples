# Stage 1: Composer dependencies installation
FROM composer:2 AS builder

# Set working directory and copy files
WORKDIR /app
COPY composer.json composer.lock ./
RUN composer install --no-dev --optimize-autoloader --no-interaction --no-progress

# Stage 2: Copy PHP application and dependencies
FROM php:8.1-apache

# Copy installed dependencies from the builder stage
COPY --from=builder /app/vendor /var/www/html/vendor

# Copy application code
COPY . /var/www/html

# Expose the port
EXPOSE 80

# Apache will run by default
