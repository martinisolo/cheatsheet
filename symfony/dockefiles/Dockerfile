FROM php:8.2-apache

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
&& apt-get install -y nano zip unzip libzip-dev locales dialog apt-utils libpng-dev curl git libicu-dev \
&& docker-php-ext-configure intl \
&& docker-php-ext-install intl zip mysqli pdo pdo_mysql gd exif \
&& docker-php-ext-enable pdo_mysql

RUN mv /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf.bak
COPY apache2.conf  /etc/apache2/sites-available/000-default.conf

# Instalamos composer, si utilizamos algun framework como symfony nos hara falta
RUN curl https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer

# Nodejs 20
RUN apt-get update && \
apt-get install -y ca-certificates curl gnupg && \
mkdir -p /etc/apt/keyrings && \
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg && \
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list && \
apt-get update && \
apt-get install nodejs -y

RUN curl -sS https://get.symfony.com/cli/installer | bash && mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

# Locale
RUN sed -i -e 's/# es_ES.UTF-8 UTF-8/es_ES.UTF-8 UTF-8/' /etc/locale.gen && \
dpkg-reconfigure --frontend=noninteractive locales && \
update-locale LANG=es_ES.UTF-8

ENV LANG es_ES.UTF-8

RUN a2enmod rewrite

# Le decimos que escuche en el puerto 80 para el servidor Web
EXPOSE 80