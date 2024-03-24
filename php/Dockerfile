FROM php:8.2-fpm-alpine

ARG USER=1000
ARG PASSWD=123456

# non-dev dependencies
RUN set -eux \
  && apk add --no-cache \
  git vim zip unzip bash curl tzdata icu-libs \
  c-client make ca-certificates imap gmp gettext libssh2 \
  libintl libxslt libpng libwebp libjpeg-turbo freetype imap \
  linux-headers oniguruma libxslt libpq vips \
  gmp libzip libxml2 freetds

# dependencies
RUN set -eux \
  && apk add --no-cache --virtual .build_deps \
  libpng-dev libwebp-dev libjpeg-turbo-dev freetype-dev imap-dev \
  linux-headers oniguruma-dev libxslt-dev vips-dev \
  libssh2-dev gmp-dev libzip-dev libxml2-dev freetds-dev \
  $PHPIZE_DEPS \
  \
  # Php extensions
  && docker-php-ext-install \
  mysqli \
  pdo_mysql \
  bcmath \
  mbstring \
  xml \
  gd \
  exif \
  zip \
  intl \
  xsl \
  pcntl \
  sockets \
  sysvmsg \
  sysvsem \
  sysvshm \
  opcache \
  imap \
  gmp \
  \
  # xdebug
  && pecl install -o -f xdebug \
  && docker-php-ext-enable xdebug \
  \
  # redis
  && pecl install -o -f redis \
  && docker-php-ext-enable redis \
  \
  # ssh2 \
  && pecl install -o -f ssh2 \
  && docker-php-ext-enable ssh2 \
  \
  # clean php extensions
  && rm -rf /tmp/pear \
  && docker-php-source delete \
  \
  # clean up \
  && apk del --purge .build_deps \
  && rm -rf /var/cache/apk/*

# install composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# set timezone
ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# replace www-data with app user
RUN sed -i \
    -e 's/^user = www-data*/user = app/' \
    -e 's/^group = www-data*/group = app/' \
    -e 's/^;listen.owner = www-data*/listen.owner = app/' \
    -e 's/^;listen.group = www-data*/listen.group = app/' \
    /usr/local/etc/php-fpm.d/www.conf

# create user/group 'app' & set perms
RUN addgroup -g ${USER} app \
    && adduser -h /home/app -G app -u ${USER} app;echo "app:${PASSWD}" | chpasswd \
    && mkdir -p /var/www/app /sock /var/log/php \
    && chown -R app:app /var/www/app /usr/local/etc /sock /var/log/php

# user/group is app
USER app:app

# go to app dir
WORKDIR /var/www/app

# expose port 9000
EXPOSE 9000
