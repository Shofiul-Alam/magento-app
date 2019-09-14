FROM php:7.1.24-fpm-alpine3.8 as magento2-php-base

ARG COMMIT="local-build"
ARG DATE="2019-09-01T00:00:00Z"
ARG URL="https://github.com/Shofiul-Alam/"
ARG VERSION="1.1"

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.build-date=$DATE \
    org.label-schema.vendor="Shofiul alam" \
    org.label-schema.name="shofiul/magento2" \
    org.label-schema.description="A PHP container with Magento 2" \
    org.label-schema.version="$VERSION" \
    org.label-schema.vcs-url=$URL \
    org.label-schema.vcs-ref=$COMMIT

ARG MAGENTO_USER_ID=1000
ARG MAGENTO_GROUP_ID=1000

RUN set -ex; \
    apk add composer; \
    apk add bash

RUN set -ex; \
    addgroup -g "${MAGENTO_GROUP_ID}" -S magento; \
    adduser -u "${MAGENTO_USER_ID}" -D -S -s /bin/bash -G magento magento; \
		adduser magento www-data; \
		sed -i '/^magento/s/!/*/' /etc/shadow; \
		echo "date.timezone=${PHP_TIMEZONE:-UTC}" > $PHP_INI_DIR/conf.d/date_timezone.ini \
 		&& echo "memory_limit=${PHP_MEMORY_LIMIT:-2G}" > $PHP_INI_DIR/conf.d/memory_limit.ini \
 		&& echo "max_execution_time=${PHP_MAX_EXECUTION_TIME:-1800}" > $PHP_INI_DIR/conf.d/max_execution_time.ini \
 		&& echo "zlib.output_compression=${PHP_ZLIB_COMPRESSION:-On}" > $PHP_INI_DIR/conf.d/zlib_compression.ini

COPY php/config/opcache.ini $PHP_INI_DIR/conf.d/

RUN set -ex; \
    apk add --update --no-cache -t .php-rundeps \
        icu-libs \
		freetype \
        libmcrypt \
        libxslt \
        libjpeg-turbo \
        libpng \
        libxml2 \
    && apk add --update --no-cache -t .build-deps \
        icu-dev \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libmcrypt-dev \
        libxml2-dev \
        libxslt-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) \
        bcmath \
        gd \
        intl \
        mcrypt \
        opcache \
        pdo_mysql \
        soap \
        xsl \
        zip \
    && docker-php-source delete; \
    apk del --purge .build-deps

ARG MAGENTO_VERSION=2.3.0

RUN set -ex; \
    rm -R /var/www/html

ADD magento2-app.tar.gz /var/www/

VOLUME /var/www/html

COPY php/config/remove_redis_config.php php/config/set_redis_config php/config/set_database_config php/docker-php-entrypoint /usr/local/bin/



RUN  set -ex; \
   chmod u+x /usr/local/bin/docker-php-entrypoint \
   && chmod 755 \
     /usr/local/bin/remove_redis_config.php \
     /usr/local/bin/set_redis_config \
     /usr/local/bin/set_database_config

WORKDIR /var/www/html

RUN set -ex; \
    find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} + \
	&& find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} + \
  	&& chown -R magento:www-data . \
	&& chmod u+x bin/magento \





