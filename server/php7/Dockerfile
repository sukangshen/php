FROM php:7.1-fpm

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib" >/etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list

# Install modules
RUN apt-get update
RUN apt-get install -y \
        git \
        curl \
        openssl \
        wget \
        vim \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libicu-dev \
             --no-install-recommends

#install extension
RUN docker-php-ext-install zip \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install bcmath

#install gd
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

#install swoole
RUN wget http://pecl.php.net/get/swoole-2.1.1.tgz \
    && pecl install swoole-2.1.1.tgz \
    && rm -f swoole-2.1.1.tgz \
    && docker-php-ext-enable swoole


# redis
RUN wget http://pecl.php.net/get/redis-3.1.6.tgz \
    && pecl install redis-3.1.6.tgz \
    && rm -f redis-3.1.6.tgz \
    && docker-php-ext-enable redis

# Install composer && global asset plugin
ENV COMPOSER_HOME /root/.composer
ENV PATH /root/.composer/vendor/bin:$PATH
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

RUN composer global require "fxp/composer-asset-plugin:^1.4.1"



RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
#RUN apt-get purge -y g++ \
#    && apt-get autoremove -y \
#    && rm -r /var/lib/apt/lists/* \
#    && rm -rf /tmp/*

RUN usermod -u 1000 www-data

EXPOSE 9000
CMD ["php-fpm"]
