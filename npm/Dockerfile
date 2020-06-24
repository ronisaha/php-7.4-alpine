FROM php:7.4.7-cli-alpine

MAINTAINER Roni Kumar Saha <roni.csed@gmail.com>

#install Exetentions and dependencies
RUN set -ex; \
	\
	apk add --no-cache --virtual .build-deps \
		libjpeg-turbo-dev \
		libpng-dev \
		libzip-dev \
		icu-dev \
		gettext-dev \
		$PHPIZE_DEPS \
		imagemagick-dev libtool\
		rabbitmq-c-dev\
		libxslt-dev libgcrypt-dev \
		postgresql-dev openldap-dev yaml-dev \
	; \
	\
	apk add --no-cache \
        imagemagick gettext nodejs nodejs-npm; \
	pecl install imagick; \
	pecl install apcu; \
	pecl install yaml; \
	pecl install redis; \
	pecl install amqp; \
	pecl install xdebug; \
	docker-php-ext-configure xsl; \
	docker-php-ext-configure intl; \
	docker-php-ext-configure gd; \
	docker-php-ext-install gd pdo pdo_pgsql pdo_mysql opcache zip exif xsl ldap intl gettext iconv; \
	docker-php-ext-enable imagick apcu yaml redis amqp xdebug; \
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add $runDeps ; \
	apk del .build-deps ; \
	rm -rf /tmp/* /usr/local/lib/php/doc/* /var/cache/apk/*

## Set php configuration
RUN { \
       echo 'date.timezone=Asia/Dhaka'; \
    } > /usr/local/etc/php/conf.d/00-override.ini ;

RUN curl -sS https://getcomposer.org/installer | php &&\
    chmod +x composer.phar &&\
    mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER=1
