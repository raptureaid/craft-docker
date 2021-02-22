FROM raptureaid/php-prod-base

# Install packages
RUN apt-get update \
    # apt Debian packages
    && apt-get install -y \
        gifsicle \
        jpegoptim \
        mariadb-client \
        nano \
        optipng \
        webp \
    # install PHP extensions
    && docker-php-ext-install \
        pdo_mysql \
    # install Composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer \
    # clean apt repo caches that don't need to be part of the image
    && apt-get clean \
    # clean directories that don't need to be part of the image
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www/project

COPY ./run_queue.sh .
RUN chmod a+x run_queue.sh

# Create storage directory and make writeable
RUN mkdir -p /var/www/project/cms/storage && \
    mkdir -p /var/www/project/cms/storage/runtime && \
    chown -R www-data:www-data /var/www/project/cms/storage

# Create cpresources directory and make writeable
RUN mkdir -p /var/www/project/cms/web/cpresources && \
    chown -R www-data:www-data /var/www/project/cms/web/cpresources

WORKDIR /var/www/project/cms

# Force permissions, update Craft, and start php-fpm

# Do a `composer install` without running any Composer scripts
# - If `composer.lock` is present, it will install what is in the lock file
# - If `composer.lock` is missing, it will update to the latest dependencies
#   and create the `composer.lock` file
# This automatic running adds to the startup overhead of `docker-compose up`
# but saves far more time in not having to deal with out of sync versions
# when working with teams or multiple environments
CMD composer install --verbose --no-progress --no-scripts --optimize-autoloader --no-interaction \
    && chown -R www-data:www-data /var/www/project/cms/config \
    && chown -R www-data:www-data /var/www/project/cms/storage \
    && chown -R www-data:www-data /var/www/project/cms/vendor \
    && chown -R www-data:www-data /var/www/project/cms/web \
    && composer craft-update \
    && php-fpm