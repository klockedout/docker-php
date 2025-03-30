# Base image
FROM php:8.1-fpm-alpine

RUN docker-php-ext-install pdo pdo_mysql

# allow super user - set this if you use Composer as a
# super user at all times like in docker containers
ENV COMPOSER_ALLOW_SUPERUSER=1

# obtain composer using multi-stage build
# https://docs.docker.com/build/building/multi-stage/
COPY --from=composer:2.4 /usr/bin/composer /usr/bin/composer

# Only copies composer.json and composer.lock first. This allows Docker to cache the install layer unless dependencies change
# https://medium.com/@softius/faster-docker-builds-with-composer-install-b4d2b15d0fff
COPY ./app/composer.* ./

# install
# Installs only production dependencies, skips scripts, and disables prompts and output. This keeps the image lean and fast.
RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

# copy application files to the working directory
# copies the actual app files into the image
COPY ./app .

# run composer dump-autoload --optimize
RUN composer dump-autoload --optimize