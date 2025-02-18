ARG PHP_VERSION=8.0
FROM php:${PHP_VERSION}-cli-alpine as php_build

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions \
    && apk add --update binutils \
    && install-php-extensions \
        ast \
        grpc \
        xdebug \
        zip \
        pcntl \
        intl \
        @composer \
        open-telemetry/opentelemetry-php-instrumentation@main \
     \
    # strip debug symbols from extensions to reduce size
    && find /usr/local/lib/php/extensions -name "*.so" -exec strip --strip-debug {} \;

FROM php_build

WORKDIR /usr/src/myapp

RUN   apk add --no-cache bash git \
      && find /usr/local/lib/php/extensions -type d -exec chmod +x -R {} \; \
      && addgroup -g "1000" -S php \
      && adduser --system \
        --gecos "" \
        --ingroup "php" \
        --uid "1000" \
        "php"

USER php
