FROM alpine
LABEL maintainer Tom Gamull <tom.gamull@gmail.com>

# For nginx/php/fpm on alpine
# https://wiki.alpinelinux.org/wiki/Nginx_with_PHP#Test_configuration

RUN apk update \
 && apk add nginx

RUN apk add php7-fpm php7-mcrypt php7-soap php7-openssl php7-gmp \
            php7-pdo_odbc php7-json php7-dom php7-pdo php7-zip \
            php7-mysqli php7-sqlite3 php7-apcu php7-pdo_pgsql \
            php7-bcmath php7-gd php7-odbc php7-pdo_mysql php7-pdo_sqlite \
            php7-gettext php7-xmlreader php7-xmlrpc php7-bz2 php7-iconv \
            php7-pdo_dblib php7-curl php7-ctype

RUN echo "@edge http://dl-4.alpinelinux.org/alpine/edge/community/" >> \
        /etc/apk/repositories \
    && apk add --no-cache zoneminder@edge zoneminder-doc@edge \
    && apk add --no-cache nginx fcgiwrap \
    && apk del --no-cache build-deps

RUN  mkdir /run/nginx /run/php\
     && touch /run/nginx/nginx.pid \
     && chown nginx -R /run/nginx \
     && nginx

RUN mkdir /var/cache/zoneminder

VOLUME /config

EXPOSE 80

COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod 755 /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]