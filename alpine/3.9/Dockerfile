FROM alpine:3.9

RUN apk update && \
    apk add --no-cache git \
        php \
        wget \
        curl \
        php7-mbstring \
        php7-mcrypt \
        php7-pdo \
        php7-xml \
        php7-json \
        php7-session \
        php7-fpm

RUN apk add --no-cache --virtual  \
        build-dependencies \
        build-base \
        bash \
        make \
        gcc \
        php7-dev && \
    cd /opt && \
    rm -rf cphalcon && \
    git clone --depth=1 "git://github.com/phalcon/cphalcon.git" && \
    cd cphalcon/build && \
    ./install && \

    cd /opt && \
    rm -rf php-psr && \
    git clone https://github.com/jbboehr/php-psr.git && \
    cd php-psr && \
    phpize && \
    ./configure && \
    make && make install && \

    echo -e "extension=psr.so \nextension=phalcon.so" > /etc/php7/conf.d/00_phalcon.ini && \

    apk del build-dependencies \
        build-base \
        make \
        gcc \
        php7-dev && \
    rm -rf /tmp/*

RUN sed -e 's/;clear_env = no/clear_env = no/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -i 's/127.0.0.1:9000/0.0.0.0:9000/g' /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;catch_workers_output = yes/catch_workers_output = yes/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;php_flag\[display_errors\] = off/php_flag\[display_errors\] = on/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;php_admin_value\[error_log\] = \/var\/log\/php7\/\$pool.error.log/php_admin_value\[error_log\] = \/var\/log\/php7\/\$pool.error.log/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;php_admin_flag\[log_errors\] = on/php_admin_flag\[log_errors\] = on/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;php_admin_value\[memory_limit\] = 32M/php_admin_value\[memory_limit\] = 128M/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;pm = dynamic/pm = static/' -i /etc/php7/php-fpm.d/www.conf && \
    sed -e 's/;pm\.max_children = 5/pm\.max_children = 10/' -i /etc/php7/php-fpm.d/www.conf

EXPOSE 9000

CMD php-fpm7 -F -R

