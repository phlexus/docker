FROM ubuntu:18.04

WORKDIR /srv

RUN apt update && \
    apt -y install software-properties-common && \
    apt update && \
    apt -y upgrade

RUN DEBIAN_FRONTEND=noninteractive apt install -y git \
        wget \
        curl \
        unzip \
        nano \
        mysql-client \
        php7.2-common \
        php7.2-cli \
        php7.2-mbstring \
        php7.2-xml \
        php7.2-curl \
        php7.2-mysql \
        php7.2-json \
        php7.2-gd \
        php7.2-zip && \
        mkdir -p /run/php

RUN wget https://github.com/composer/composer/releases/download/2.0.8/composer.phar && \
    wget https://github.com/phalcon/zephir/releases/download/0.12.20/zephir.phar && \
    chmod ugo+x composer.phar zephir.phar && \
    mv composer.phar /usr/local/bin/composer && \
    mv zephir.phar /usr/local/bin/zephir

RUN apt install -y \
        make \
        gcc \
        re2c \
        autoconf \
        automake \
        build-essential \
        php7.2-dev && \
        cd /opt && \
        rm -rf php-zephir-parser && \
        git clone git://github.com/phalcon/php-zephir-parser.git && \
        cd php-zephir-parser && \
        phpize && \
        ./configure && \
        make -j`nproc` && make install && \
        echo 'extension=zephir_parser.so' > /etc/php/7.2/cli/conf.d/48-zephir_parser.ini && \
        cd /opt && \
        rm -rf php-psr && \
        git clone --depth=1 https://github.com/jbboehr/php-psr.git && \
        cd php-psr && \
        phpize && \
        ./configure && \
        make -j`nproc` && make install && \
        echo 'extension=psr.so' > /etc/php/7.2/cli/conf.d/49-psr.ini && \
        cd /opt && \
        rm -rf cphalcon && \
        git clone --depth=1 --branch "v4.1.0" "https://github.com/phalcon/cphalcon.git" && \
        cd cphalcon && \
        zephir fullclean && \
        zephir build && \
        cd ext/ && \
        ./install && \
        apt purge -y make \
        gcc \
        re2c \
        autoconf \
        automake \
        build-essential \
        php7.2-dev && \
        apt clean

ADD ini/50-phalcon.ini /etc/php/7.2/cli/conf.d

CMD mkdir -p /srv/public && php -S 0.0.0.0:80 -t public/
