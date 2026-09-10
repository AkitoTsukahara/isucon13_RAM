FROM php:8.2.11-fpm-bullseye

WORKDIR /tmp
ENV DEBIAN_FRONTEND=noninteractive
# bullseye is EOL: deb.debian.org no longer serves its packages and the Release
# files are expired, so use the snapshot repositories pinned to the base image.
RUN echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid-until && \
    sed -i \
      -e 's|^deb http://deb.debian.org|# deb http://deb.debian.org|' \
      -e 's|^# deb http://snapshot.debian.org|deb http://snapshot.debian.org|' \
      /etc/apt/sources.list
RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get install -y curl wget gcc g++ make sqlite3 locales locales-all git zip && \
    wget -q https://dev.mysql.com/get/mysql-apt-config_0.8.35-1_all.deb && \
    apt-get -y install ./mysql-apt-config_0.8.35-1_all.deb && \
    apt-get -y update && \
    apt-get -y install default-mysql-client pdns-server pdns-backend-mysql

RUN rm -f /etc/powerdns/pdns.d/bind.conf

RUN docker-php-ext-install pdo_mysql

RUN locale-gen en_US.UTF-8
RUN useradd --uid=1001 --create-home isucon
USER isucon

WORKDIR /home/isucon/webapp/php
COPY --chown=isucon:isucon ./ /home/isucon/webapp/php/
RUN ./composer.phar install

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

ENV TZ utc
