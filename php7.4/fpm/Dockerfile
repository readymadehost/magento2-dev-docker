FROM php:7.4-fpm

LABEL maintainer="ReadyMadeHost http://readymadehost.com"

# Installing required packages
RUN apt-get update && apt-get upgrade -y
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
	apt-transport-https \
	libicu-dev \
  libonig-dev \
  libzip-dev \
  libcurl4-openssl-dev \
  pkg-config \
  libssl-dev  \
  libmcrypt-dev \
  libxml2-dev \
  libxslt-dev \
  libbz2-dev \
  libfreetype6-dev \
  libjpeg-dev \
  libpng-dev \
  openssl \
  gnupg2

RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg

# Installing extensions
RUN docker-php-ext-install bcmath \
  ctype \
  curl \
  dom \
  iconv \
  intl \
  mbstring \
  pdo_mysql \
  simplexml \
  soap \
  xsl \
  zip \
  sockets \
  gd

# Installing other extensions but enabled dynamically
RUN pecl install redis
# RUN docker-php-ext-enable redis
RUN pecl install xdebug
# RUN docker-php-ext-enable xdebug

# Updating php.ini
ADD conf/php.ini /usr/local/etc/php/php.ini

ARG PHP_ENABLE_REDIS
ENV PHP_ENABLE_REDIS ${PHP_ENABLE_REDIS}
ARG PHP_ENABLE_XDEBUG
ENV PHP_ENABLE_XDEBUG ${PHP_ENABLE_XDEBUG}

# Add scripts
ADD docker-entrypoint.sh /root/docker-entrypoint.sh
RUN chmod +x /root/docker-entrypoint.sh

CMD ["/root/docker-entrypoint.sh", "php-fpm", "-F"]
