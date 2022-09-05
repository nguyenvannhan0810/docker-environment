FROM php:8.1-fpm-alpine

WORKDIR /var/www/html

ENV MUSL_LOCPATH="/usr/share/i18n/locales/musl"

RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/v3.13/community libpq-dev icu-dev gettext-dev gettext musl-locales musl-locales-lang
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && pecl install xdebug \
    && cd "$MUSL_LOCPATH" \
    && for i in *.UTF-8; do cp -a "$i" "${i%%.UTF-8}"; done \
    && apk del -f .build-deps

RUN docker-php-ext-configure gettext
RUN docker-php-ext-install pdo pdo_pgsql gettext
RUN docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./src .
