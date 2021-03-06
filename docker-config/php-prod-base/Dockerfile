FROM php:7.3-fpm

# Install packages
RUN apt-get update \
    # apt Debian packages
    && apt-get install -y \
        apt-utils \
        autoconf \
        ca-certificates \
        curl \
        g++ \
        libbz2-dev \
        libfreetype6-dev \
        libicu-dev \
        libjpeg62-turbo-dev \
        libmagickwand-dev \
        libpng-dev \
        libpq-dev \
        libssl-dev \
        libzip-dev \
        unzip \
        zip \
    # pecl PHP extensions
    && pecl install \
        imagick-3.4.4 \
        redis \
    # configure PHP extensions
    && docker-php-ext-configure \
        gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    # install PHP extensions
    && docker-php-ext-install \
        bcmath \
        bz2 \
        exif \
        ftp \
        gd \
        gettext \
        iconv \
        intl \
        mbstring \
        opcache \
        pdo \
        shmop \
        sockets \
        sysvmsg \
        sysvsem \
        sysvshm \
        zip \
    # enable PHP extensions
    && docker-php-ext-enable \
        imagick \
        redis \
    # clean apt repo caches that don't need to be part of the image
    && apt-get clean \
    # clean directories that don't need to be part of the image
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Append php.ini overrides to the end of the file
RUN echo "upload_max_filesize = 10M" > /usr/local/etc/php/php.ini && \
    echo "post_max_size = 10M" >> /usr/local/etc/php/php.ini && \
    echo "max_execution_time = 300" >> /usr/local/etc/php/php.ini && \
    echo "memory_limit = 256M" >> /usr/local/etc/php/php.ini && \
    echo "opcache.revalidate_freq = 0" >> /usr/local/etc/php/php.ini && \
    echo "opcache.validate_timestamps = 1" >> /usr/local/etc/php/php.ini

# Copy 'zzz-docker.config' file into place for php-fpm
COPY ./zzz-docker.conf /usr/local/etc/php-fpm.d/zzz-docker.conf