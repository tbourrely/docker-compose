FROM php:7.3.6-zts

# install the PHP extensions we need
RUN set -ex; \
	export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS"; \
	\
	savedAptMark="$(apt-mark showmanual)"; \
	\
	apt-get update; \
	apt-get install -y --no-install-recommends \
		libjpeg-dev \
		libpng-dev \
		libzip-dev \
		libxml2-dev \
		libcurl4-openssl-dev \
    		libssl-dev \ 
		curl \
		git \
		libmagickwand-dev \
		g++ \
		make \
		autoconf \
	; \
	pecl install imagick; \
#	pecl install pthreads; \
        
	curl -sSL https://github.com/krakjoe/pthreads/archive/master.zip -o /tmp/pthreads.zip \
        && unzip /tmp/pthreads.zip -d /tmp \
        && cd /tmp/pthreads-* \
        && phpize \
        && ./configure \
        && make \
        && make install \
        && rm -rf /tmp/pthreads*; \
 	curl --silent --show-error https://getcomposer.org/installer | php; \
	chmod +x composer.phar && mv composer.phar /usr/local/bin/composer; \
	\
#	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
#	docker-php-ext install gd; \
	docker-php-ext-install mysqli zip; \
	docker-php-ext-install pdo pdo_mysql xml curl phar; \
	docker-php-ext-enable imagick; \
	docker-php-ext-enable pthreads; \
	\
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
	apt-mark auto '.*' > /dev/null; \
	apt-mark manual $savedAptMark; \
	ldd "$(php -r 'echo ini_get("extension_dir");')"/*.so \
		| awk '/=>/ { print $3 }' \
		| sort -u \
		| xargs -r dpkg-query -S \
		| cut -d: -f1 \
		| sort -u \
		| xargs -rt apt-mark manual; \
	\
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*


#COPY config/php.ini $PHP_INI_DIR/
ADD config/php.ini $PHP_INI_DIR/
