FROM composer as composer
ENTRYPOINT ["composer"]

FROM php:8.1-fpm-alpine as php

RUN sed -i "s/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g" /etc/apk/repositories \
    && apk add --no-cache autoconf g++ libtool make curl-dev libxml2-dev gmp-dev linux-headers git libpng-dev freetype-dev libjpeg-turbo-dev libzip-dev ffmpeg imagemagick imagemagick-libs imagemagick-dev oniguruma-dev \
    && rm -rf /var/lib/apt/lists/*
RUN echo "---------- Install bcmath,sockets ----------" \
    && docker-php-ext-install -j$(nproc) bcmath sockets \
    && echo "---------- Install pdo_mysql ----------" \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && echo "---------- Install zip ----------" \
    && docker-php-ext-install -j$(nproc) zip \
    && echo "---------- Install pcntl ----------" \
    && docker-php-ext-install -j$(nproc) pcntl \
    && echo "---------- Install mysqli ----------" \
    && docker-php-ext-install -j$(nproc) mysqli \
    && echo "---------- Install mbstring ----------" \
    && docker-php-ext-install -j$(nproc) mbstring \
    && echo "---------- Install exif ----------" \
    && docker-php-ext-install -j$(nproc) exif \
    && echo "---------- Install bz2 ----------" \
    && apk add --no-cache bzip2-dev \
    && docker-php-ext-install -j$(nproc) bz2 \
    && echo "---------- Install curl ----------" \
    && docker-php-ext-install -j$(nproc) curl \
    && echo "---------- Install gd ----------" \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && echo "---------- Install redis ----------" \
    && curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/5.3.6.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-5.3.6 /usr/src/php/ext/redis \
    && docker-php-ext-install -j$(nproc) redis \
    && echo "---------- Install gmp ----------" \
    && docker-php-ext-install -j$(nproc) gmp \
    && echo "---------- Install opcache ----------" \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache \
    && rm -rf /var/lib/apt/lists/* \

WORKDIR /www
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN apk update \
    && apk add --no-cache python3 python3-dev tzdata vim zsh \
    && sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && echo "export PATH=$PATH:/root/.composer/vendor/bin" >> /root/.zshrc \
    && echo "alias phpunit='./vendor/bin/phpunit'" >> /root/.zshrc
