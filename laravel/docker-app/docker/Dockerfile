FROM php:7.2-apache-stretch

RUN  docker-php-ext-install pdo_mysql \
    # && pecl install xdebug \
    # $$ docker-php-ext-enable xdebug \
    # $$ service apache2 restart \
    && a2enmod rewrite

RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.6.1.tgz' -o xdebug.tar.gz \
    && mkdir -p xdebug \
    && tar -xf xdebug.tar.gz -C xdebug --strip-components=1 \
    && rm xdebug.tar.gz \
    && ( \
    cd xdebug \
    && phpize \
    && ./configure --enable-xdebug \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r xdebug \
    && docker-php-ext-enable xdebug   

COPY . /var/www/html
COPY docker/vhost.conf  /etc/apache2/sites-available/000-default.conf
COPY docker/config/php/conf.d/*.ini /usr/local/etc/php/conf.d

RUN chown -R www-data:www-data /var/www/html