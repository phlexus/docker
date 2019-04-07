FROM ubuntu:18.04

RUN apt update && \
    DEBIAN_FRONTEND=noninteractive apt install -y git \
        wget \
        curl \
        php7.2-common \
        php7.2-cli \
        php7.2-mbstring \
        php7.2-xml \
        php7.2-fpm \
        php7.2-zip \
        php7.2-xml

RUN apt install -y  \
        make \
        gcc \
        php7.2-dev && \
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

    echo -e "extension=psr.so \nextension=phalcon.so" > /etc/php/7.2/mods-available/00_phalcon.ini && \

    apt purge -y make \
        gcc \
        php7.2-dev && \
    apt clean

RUN sed -e 's/;clear_env = no/clear_env = no/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -i 's/127.0.0.1:9000/0.0.0.0:9000/g' /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;catch_workers_output = yes/catch_workers_output = yes/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;php_flag\[display_errors\] = off/php_flag\[display_errors\] = on/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;php_admin_value\[error_log\] = \/var\/log\/php7\/\$pool.error.log/php_admin_value\[error_log\] = \/var\/log\/php7\/\$pool.error.log/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;php_admin_flag\[log_errors\] = on/php_admin_flag\[log_errors\] = on/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;php_admin_value\[memory_limit\] = 32M/php_admin_value\[memory_limit\] = 128M/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;pm = dynamic/pm = static/' -i /etc/php/7.2/fpm/php-fpm.conf && \
    sed -e 's/;pm\.max_children = 5/pm\.max_children = 10/' -i /etc/php/7.2/fpm/php-fpm.conf

EXPOSE 9000 80 443

CMD php-fpm7 -F -R
