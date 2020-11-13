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
        nginx \
        supervisor \
        mysql-client \
        php7.2-fpm \
        php7.2-common \
        php7.2-cli \
        php7.2-mbstring \
        php7.2-xml \
        php7.2-curl \
        php7.2-mysql \
        php7.2-json \php7.2-zip && \
        mkdir -p /run/php

RUN wget https://github.com/composer/composer/releases/download/1.9.3/composer.phar && \
    wget https://github.com/phalcon/zephir/releases/download/0.12.16/zephir.phar && \
    chmod ugo+x composer.phar zephir.phar && \
    mv composer.phar /usr/local/bin/composer && \
    mv zephir.phar /usr/local/bin/zephir && \
    composer global require hirak/prestissimo

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
        make && make install && \
        echo 'extension=zephir_parser.so' > /etc/php/7.2/cli/conf.d/48-zephir_parser.ini && \
        cd /opt && \
        rm -rf php-psr && \
        git clone --depth=1 https://github.com/jbboehr/php-psr.git && \
        cd php-psr && \
        phpize && \
        ./configure && \
        make && make install && \
        echo 'extension=psr.so' > /etc/php/7.2/cli/conf.d/49-psr.ini && \
        cd /opt && \
        rm -rf cphalcon && \
        git clone --depth=1 --branch "4.0.x" "https://github.com/phalcon/cphalcon.git" && \
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

EXPOSE 9000 80

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord"]
