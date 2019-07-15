FROM php:7.1-fpm

MAINTAINER Aleksej Burdash <working.unit.0@icloud.com>

WORKDIR "/application"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

ENV APT_NODE_DEPS \
    gnupg 

ENV APT_PHP_DEPS \
     libfreetype6-dev \
     libjpeg62-turbo-dev \
     libmcrypt-dev \
     libpng-dev \
     libzip-dev \
     libpq-dev \
     zlib1g-dev \
     libicu-dev \
     libmagickwand-dev

ENV PHP_STD_MOD \
    iconv \
    mcrypt \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    opcache \
    zip \
    exif \
    intl \
    pcntl \
    bcmath \
    sockets \
    zip 

ENV PECL_MOD \
    apcu \
    redis \
    xdebug \
    imagick

#NODE INSTALL 
RUN apt-get update \
    && apt-get install -y $APT_NODE_DEPS --no-install-recommends \
    && curl -sL https://deb.nodesource.com/setup_8.x | bash - \
	&& apt-get install -y nodejs --no-install-recommends 

#COMPOSER INSTALL 
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --version=1.6.5 --filename=composer --install-dir=/bin

#PHP_MOD INSTALL
RUN apt-get install -y $APT_PHP_DEPS --no-install-recommends \
    && docker-php-ext-install -j$(nproc) $PHP_STD_MOD \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

#PECL_MOD INSTALL
RUN pecl install $PECL_MOD \
    && docker-php-ext-enable redis apcu xdebug imagick\
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
