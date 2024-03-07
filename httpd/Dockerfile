FROM httpd:2.4-alpine

ARG USER

# replace port & servername
RUN sed -i \
    -e 's/Listen 80/Listen 8000/' \
    -e 's/^#ServerName.*/ServerName localhost:8000/' \
    /usr/local/apache2/conf/httpd.conf

# uncomment proxy & proxy_fcgi modules
RUN sed -i \
    -e 's/^#\(LoadModule proxy_module modules\/mod_proxy.so\)/\1/' \
    -e 's/^#\(LoadModule proxy_fcgi_module modules\/mod_proxy_fcgi.so\)/\1/' \
    /usr/local/apache2/conf/httpd.conf

# copy vhost default
COPY default.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf

# replace vhost
RUN sed -i \
    -e 's/#Include\ conf\/extra\/httpd-vhosts.conf/Include\ conf\/extra\/httpd-vhosts.conf/' \
    /usr/local/apache2/conf/httpd.conf

# create user/group 'app' + log dir & set perms
RUN addgroup -g ${USER} app \
    && adduser -D -H -h /home/app -G app -u ${USER} app \
    && mkdir -p /var/www/app /usr/local/apache2/logs \
    && chown -R app:app /var/www/app /usr/local/apache2/logs

# user/group is app
USER app:app

# go to web dir
WORKDIR /var/www/app/public

# expose port 8000
EXPOSE 8000

# start httpd
CMD ["httpd-foreground"]
