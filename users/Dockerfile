FROM php:7.2-fpm

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update && apt-get install -y apt-utils libmcrypt-dev \
    sqlite3 libsqlite3-dev libmagickwand-dev zip unzip git  \
    libpcre3-dev \
    libmemcached-dev \
    --no-install-recommends

RUN pecl install mcrypt-1.0.1 \
&& docker-php-ext-enable mcrypt

RUN cd /usr/local \
&& git clone -b 3.4.x  --depth=1 "git://github.com/phalcon/cphalcon.git" \
&& git clone --depth=1 "git://github.com/phalcon/phalcon-devtools.git" \
&& cd cphalcon/build \
&& ./install \
&& docker-php-ext-enable phalcon \
&& ln -s /usr/local/phalcon-devtools/phalcon /usr/local/bin/phalcon \
&& chmod ugo+x /usr/local/bin/phalcon

WORKDIR /var/www

COPY . /var/www

RUN phalcon project users --type=micro
RUN cd users
RUN composer install --prefer-dist --no-scripts --no-dev --no-autoloader && rm -rf /root/.composer