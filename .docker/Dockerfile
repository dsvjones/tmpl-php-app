FROM alpine:3.18.3

# Setup Document Root
WORKDIR /var/www/html

# Update APK and upgrade installed packages.
RUN apk update && apk upgrade
RUN apk add --no-cache bash

# Install Nginx
RUN apk add --no-cache nginx

# Intall base PHP
RUN apk add --no-cache php82 \
    php82-common \
    php82-fpm \
    php82-opcache

# Install Production Modules
RUN apk add --no-cache php82-gd \
    php82-mysqli \
    php82-zlib \
    php82-curl \
    php82-pecl-redis \
    php82-ctype \
    php82-tokenizer \
    php82-iconv \
    php82-intl \
    php82-json \
    php82-mbstring \
    php82-openssl \
    php82-session \
    php82-simplexml

# Install Dev-Only Modules
RUN apk add --no-cache php82-xdebug

# Configure Nginx
COPY nginx/nginx.conf /etc/nginx/nginx.conf
COPY nginx/fastcgi.conf /etc/nginx/fastcgi.conf
COPY nginx/conf.d /etc/nginx/conf.d

# Configure PHP-FPM
COPY php-fpm/php-fpm.conf /etc/php82/php-fpm.conf
COPY php-fpm/www.conf /etc/php82/php-fpm.d/www.conf
COPY php-fpm/php.ini /etc/php82/php.ini

# Copy PHP configuration files.
COPY php-fpm/conf.d/ /etc/php82/conf.d/

# Symlink for PHP
# RUN ls -s /usr/bin/php82 /usr/bin/php

# Copy testing PHP file.
COPY test.php /var/www/html/test.php

# Secure files and folders:
RUN chown -R nobody.nobody /var/www/html /run /var/lib/nginx /var/log/nginx

USER nobody

EXPOSE 8080
EXPOSE 8443

STOPSIGNAL SIGTERM

CMD ["/bin/bash", "-c", "php-fpm82 && chmod 777 /run/php8-fpm.sock && chmod 755 /var/www/html/* && nginx -g 'daemon off;'"]