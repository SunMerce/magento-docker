FROM php:8.3-fpm

RUN apt-get update \
  && apt-get install -y --quiet \
    cron \
    vim \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libxslt1-dev \
    libzip-dev \
    curl git

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

RUN docker-php-ext-configure \
  gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/

RUN docker-php-ext-install \
  bcmath \
  gd \
  intl \
  pdo_mysql \
  soap \
  xsl \
  opcache \
  zip \
  sockets \
  ftp
  
RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

# set www to be the same UID of host user to avoid permission issue
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

ENV PHP_MEMORY_LIMIT 2G
ENV PHP_PORT 9000
ENV PHP_PM dynamic
ENV PHP_PM_MAX_CHILDREN 10
ENV PHP_PM_START_SERVERS 4
ENV PHP_PM_MIN_SPARE_SERVERS 2
ENV PHP_PM_MAX_SPARE_SERVERS 6

COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/xdebug.ini /usr/local/etc/php/
COPY bin/magento-install /usr/local/bin/
COPY bin/start /usr/local/bin/

RUN echo "*/5 * * * * /opt/APPLICATION/restart.sh > /proc/1/fd/1 2>&1" >> /etc/cron.d/restart-cron \
    # Give the necessary rights to the user to run the cron
    && crontab -u www-data /etc/cron.d/restart-cron \
    && chmod u+s /usr/sbin/cron

# swtich to www user for safety measure
USER www-data:www-data
WORKDIR /var/www

ARG WEB_PORT
ENV MAGENTO_WEB_PORT=$WEB_PORT

CMD ["/usr/local/bin/start"]
