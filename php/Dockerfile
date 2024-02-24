FROM php:8.1-fpm-alpine

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

# create user/group 'app' + web/sock/log & set perms
RUN addgroup -g 1000 app \
    && adduser -D -H -h /var/www/html -s /sbin/nologin -G app -u 1000 app \
    && mkdir -p /var/www/html /sock /var/log/php \
    && chown -R app:app /var/www/html /usr/local/etc /sock /var/log/php

# user/group is app
USER app:app

# go to web dir
WORKDIR /var/www/html

# expose port 9000
EXPOSE 9000