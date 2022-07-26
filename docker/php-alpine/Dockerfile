FROM php:7.3-alpine3.10

MAINTAINER Maintainer.com <maintainer.com.br>
ENV created_at 26-07-2022

# # Installing dependencies
RUN apk add --update --no-cache libgd libpng-dev libjpeg-turbo-dev freetype-dev 
RUN apk add git composer php7 php7-cli php7-common php7-pdo php7-pdo_mysql php7-mysqli php7-pdo_pgsql php7-mcrypt \
    php7-pgsql php7-mbstring php7-xml php7-phar php7-gd libzip-dev php7-pear php7-opcache php7-intl postfix \
    php7-apache2 php-session apache2 apache2-utils autoconf build-base postgresql-dev

# # Installing/Configuring dependencies
RUN docker-php-ext-configure pgsql -with-pgsql==mysqlnd \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-configure mysqli --with-mysqli=mysqlnd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo \
    && docker-php-ext-install mysqli \ 
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-install session

# # Clearing package instalations 
RUN rm -rf /var/cache/apk/*

## setting php.ini
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
COPY config/php.ini /etc/php7/php.ini

## XDebug Install
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN echo "xdebug.start_with_request=yes" >> /etc/php7/conf.d/xdebug.ini \
    && echo "xdebug.mode=debug" >> /etc/php7/conf.d/xdebug.ini \
    && echo "xdebug.log=/tmp/xdebug.log" >> /etc/php7/conf.d/xdebug.ini \
    && echo "xdebug.client_host=host.docker.internal" >> /etc/php7/conf.d/xdebug.ini \
    && echo "xdebug.idekey=VSCODE" >> /etc/php7/conf.d/xdebug.ini \
    && echo "xdebug.client_port=9003" >> /etc/php7/conf.d/xdebug.ini

# # Apache RUN
COPY config/httpd.conf /etc/apache2/httpd.conf
RUN mkdir -p /run/apache2

# # Permission for everything as it is done today
RUN chmod -R 777 /var/www/html

# ## Port forwarding
EXPOSE 80
CMD /usr/sbin/httpd -D FOREGROUND
