FROM alpine:3.8

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
        php7-fpm \
        php7-openssl \
        php7-phar \
        php7-zip \
        php7-dom \
        php7-xml \
        php7-xmlwriter \
        php7-tokenizer \
        php7-pdo_sqlite

RUN apk add --no-cache --virtual  \
        build-dependencies \
        build-base \
        bash \
        make \
        gcc \
        php7-dev && \
    cd /opt && \
    rm -rf cphalcon && \
    git clone --branch '4.0.x' "git://github.com/phalcon/cphalcon.git" && \
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

EXPOSE 9000 80 443

CMD php-fpm7 -F -R
