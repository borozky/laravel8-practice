FROM php:7.4-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# install all the dependencies and enable PHP modules
# additional "docker-php-ext-install" can be found here 
# https://gist.github.com/giansalex/2776a4206666d940d014792ab4700d80

# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    supervisor

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Xdebug (optional)
ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
;fi

RUN curl -O "https://getcomposer.org/download/2.1.6/composer.phar" && chmod a+x composer.phar && mv composer.phar /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Set working directory
WORKDIR /var/www
USER root

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisor/conf.d/supervisord.conf"]
# ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisor/supervisord.conf"]
# ENTRYPOINT ["/usr/bin/supervisord"]





