FROM php:7-fpm
ARG TIMEZONE

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    libxml2-dev \
    libpng-dev \
    libssl-dev libcurl4-openssl-dev pkg-config

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

ENV COMPOSER_ALLOW_SUPERUSER 1

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

RUN curl -fsS -o /tmp/icu.tgz -L http://download.icu-project.org/files/icu4c/60.2/icu4c-60_2-src.tgz \
  && tar -zxf /tmp/icu.tgz -C /tmp \
  && cd /tmp/icu/source \
  && ./configure --prefix=/usr/local \
  && make \
  && make install \
  && rm -rf /tmp/icu*

# ENV PHP_CPPFLAGS="$PHP_CPPFLAGS -std=c++11"

RUN docker-php-ext-configure intl --with-icu-dir=/usr/local

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql ctype curl dom gd iconv intl json mysqli mbstring opcache phar session tokenizer xml zip

#   wddx  xmlreader xmlrpc xmlwriter xsl zend_test zip

# install xdebug
RUN pecl install xdebug-2.6.0

ADD php.ini /usr/local/etc/php/conf.d/docker-php.ini
ADD xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN docker-php-ext-enable xdebug

RUN rm -rf /var/cache/apk/* && rm -rf /tmp/*

RUN echo 'alias sf="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/symfony