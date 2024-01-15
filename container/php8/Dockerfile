FROM php:8.1-apache-buster

RUN apt update; apt upgrade -y \
	&& ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime \
	&& a2enmod rewrite headers expires \
	&& apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libmemcached-dev \
        zlib1g-dev \
        libxml2-dev \
        curl libcurl4-openssl-dev \
        default-mysql-client \
		libpq-dev \
        libsqlite3-dev \
        libsqlite3-0 \
        libc-client-dev \
        libkrb5-dev \
        zlib1g-dev \
		libicu-dev \
		g++ \
		vim \
		unzip \
		git \
		python \
		graphviz \
		ngrep \
		wget \
		gnupg \
		tcpdump \
		net-tools \
		libonig-dev \
		libzip-dev


RUN docker-php-ext-configure gd \
	&& docker-php-ext-install -j$(nproc) \
		gd \
		simplexml \
		exif \
		curl \
		mbstring \
		soap \
		pdo \
		pdo_mysql \
		pdo_pgsql \
		pdo_sqlite \
		mysqli \
		calendar \
		bcmath \
		zip \
		intl \
		iconv

RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) imap

RUN pecl channel-update pecl.php.net && pecl install xdebug-3.2.1
RUN docker-php-ext-enable xdebug

# Memcache
RUN pecl install memcached-3.2.0
RUN docker-php-ext-enable memcached
RUN pecl install memcache-8.0
RUN docker-php-ext-enable memcache

RUN apt install -y libldb-dev libldap2-dev \
	&& docker-php-ext-install -j$(nproc) ldap

RUN curl -sS https://getcomposer.org/installer  |  php -- --install-dir=/usr/bin/ --filename=composer

RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo "post_max_size = 100M" >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo "upload_max_filesize = 100M" >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo "short_open_tag = Off" >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo "expose_php = On" >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo 'date.timezone="America/Sao_Paulo"' >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo 'error_reporting = E_ALL' >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo 'log_errors = On' >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo 'always_populate_raw_post_data=-1' >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo 'memory_limit=512M' >> /usr/local/etc/php/conf.d/customs.ini \
	&& echo 'error_log=syslog' >> /usr/local/etc/php/conf.d/customs.ini 

# Driver SQL Server
RUN apt-get install -y unixodbc-dev
RUN pecl install sqlsrv && pecl install pdo_sqlsrv
RUN printf "extension=sqlsrv.so\n" > /usr/local/etc/php/conf.d/sqlsrv.ini
RUN printf "extension=pdo_sqlsrv.so\n" > /usr/local/etc/php/conf.d/pdo_sqlsrv.ini
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install -y msodbcsql18
RUN ACCEPT_EULA=Y apt-get install -y mssql-tools18
RUN echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bashrc
RUN apt-get install -y unixodbc-dev

# Redis
RUN pecl install -o -f redis \
	&&  docker-php-ext-enable redis


RUN rm -Rf  /var/cache/apt/ &&  rm -rf /tmp/pear