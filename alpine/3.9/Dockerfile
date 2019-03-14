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
        php7-session

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
        php7-dev
