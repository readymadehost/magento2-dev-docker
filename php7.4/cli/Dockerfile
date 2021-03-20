FROM php:7.4-cli

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
  gnupg2 \
  git \
  vim \
  zip \
  unzip \
  wget \
  screenfetch \
  mariadb-client

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

# Installing composer
VOLUME /root/.composer/cache
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installing n98-magerun2
RUN curl -O https://files.magerun.net/n98-magerun2.phar && chmod +x ./n98-magerun2.phar && mv ./n98-magerun2.phar /usr/local/bin/

ARG NODE_VERSION
ENV NODE_VERSION ${NODE_VERSION}

# Installing node
RUN curl -sL https://deb.nodesource.com/setup_${NODE_VERSION} | bash -
RUN apt-get -y install nodejs

# Installing yarn and grunt-cli using npm
RUN npm install --global yarn
RUN npm install --global grunt-cli

# Manage project and add scripts
RUN mkdir /var/www/project
ADD manage-project-permission.sh /root/manage-project-permission.sh
RUN chmod +x /root/manage-project-permission.sh
RUN echo 'alias mpp="/root/manage-project-permission.sh"' >> /root/.bashrc
RUN /root/manage-project-permission.sh

ARG PHP_ENABLE_REDIS
ENV PHP_ENABLE_REDIS ${PHP_ENABLE_REDIS}
ARG PHP_ENABLE_XDEBUG
ENV PHP_ENABLE_XDEBUG ${PHP_ENABLE_XDEBUG}

ADD docker-entrypoint.sh /root/docker-entrypoint.sh
RUN chmod +x /root/docker-entrypoint.sh

CMD ["/root/docker-entrypoint.sh", "bash"]
