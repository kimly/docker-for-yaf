FROM php:7.2.2-fpm-alpine3.7

MAINTAINER Panda <kimlyfly@gmail.com>

RUN echo http://mirrors.aliyun.com/alpine/v3.7/main > /etc/apk/repositories; \
    echo http://mirrors.aliyun.com/alpine/v3.7/community >> /etc/apk/repositories \
  && apk update \
  && apk add --no-cache \
  # 注：alpine系统必须添加此项，PECL才能正常工作
  $PHPIZE_DEPS \
  linux-headers autoconf libmcrypt libmcrypt-dev  libpng-dev postgresql-dev freetype-dev libpng-dev libjpeg-turbo-dev freetype libpng libjpeg-turbo \
   # php >=7.0 (use libvpx for php <7.0)
   libwebp-dev \
  && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
  && docker-php-ext-configure gd \
    --with-jpeg-dir=/usr/include --with-png-dir=/usr/include --with-webp-dir=/usr/include --with-freetype-dir=/usr/include \
  && docker-php-ext-install gd pdo_mysql pdo_pgsql mysqli sockets opcache zip\
  && pecl install yaf \
  && pecl install xdebug-2.6.0 \
  && pecl install redis \
  && pecl install mcrypt-1.0.1 \
  && pecl install mongodb \
  && rm -rf /tmp/pear \
  && docker-php-ext-enable xdebug redis yaf mcrypt mongodb \
  && apk del --no-cache linux-headers autoconf libmcrypt libmcrypt-dev libpng-dev postgresql-dev

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer self-update

