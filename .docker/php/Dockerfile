FROM php:8.3-fpm

ARG XDEBUG_CLIENT_HOST
ENV XDEBUG_CLIENT_HOST=${XDEBUG_CLIENT_HOST}

# Set environment variables
ENV COMPOSER_HOME /logs
ENV PHP_TIMEZONE Europe/Paris

# Update package list and install system dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    apt-utils \
    git \
    zip \
    openssh-client \
    libzip-dev \
    libpq-dev \
    libicu-dev \
    g++ \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libxpm-dev \
    libvpx-dev \
    libxml2-dev \
    libwebp-dev

# Install PHP extensions
RUN docker-php-ext-install \
    zip \
    pdo \
    pdo_pgsql \
    pdo_mysql \
    intl \
    opcache \
    gd

# Configure PHP settings
RUN echo "memory_limit=-1" > "$PHP_INI_DIR/conf.d/memory-limit.ini" \
 && echo "date.timezone=${PHP_TIMEZONE:-Europe/Paris}" > "$PHP_INI_DIR/conf.d/date_timezone.ini"
# && echo "xdebug.mode=develop, coverage, debug, gcstats, profile, trace"  > "/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini"  \
# && echo "xdebug.idekey=PHPSTORM"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
# && echo "xdebug.client_host=$XDEBUG_CLIENT_HOST" >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" \
# && echo "xdebug.client_port = 9003" >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini" \
# && echo "xdebug.log=/var/logs/xdebug.log"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
## && echo "xdebug.output_dir = /log"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
# && echo "xdebug.start_with_request=yes"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
## && echo "start_with_request=default"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
# && echo "session.auto_start=1"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
# && echo "zend_extension=xdebug"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"   \
# && echo "xdebug.discover_client_host=0"  >> "$PHP_INI_DIR/conf.d/docker-php-ext-xdebug.ini"

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-configure intl
# ADD Xdepug
RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN { \
        echo 'zend_extension=xdebug.so'; \
        echo 'xdebug.mode=debug'; \
        echo 'xdebug.start_with_request=yes'; \
        echo 'xdebug.client_host=${XDEBUG_CLIENT_HOST}'; \
        echo 'xdebug.client_port=9003'; \
        echo 'xdebug.log=/var/logs/xdebug.log'; \
        echo 'xdebug.max_nesting_level=256'; \
        echo 'xdebug.idekey=PHPSTORM'; \
        echo 'xdebug.discover_client_host=0'; \
    } > /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini



# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

