FROM wordpress:php7.1-fpm

RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng12-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    gettext-base \
  && rm -rf /var/lib/apt/lists/*

# Set the PHP redis version
ENV PHPREDIS_VERSION php7

# Install the PHP mcrypt extention
RUN docker-php-ext-install mcrypt \
  # Install the PHP gd library
  && docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 \
  && docker-php-ext-install gd \
  ## Install the PHP redis extension
  && curl -L -o redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
  && tar xfz redis.tar.gz \
  && rm -r redis.tar.gz \
  && ( \
        cd phpredis-$PHPREDIS_VERSION \
        && phpize \
        && ./configure \
        && make \
        && make install \
    ) \
  && rm -r phpredis-$PHPREDIS_VERSION \
  && docker-php-ext-enable redis \
  # Install the PHP bcmath extention
  && docker-php-ext-install bcmath \
  # Install the PHP zip extention
  && docker-php-ext-install zip

ADD conf/uploads.tmpl /opt/uploads.tmpl

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]

CMD php-fpm
