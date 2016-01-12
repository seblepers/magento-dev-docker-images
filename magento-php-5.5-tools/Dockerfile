FROM php:5.5-cli

MAINTAINER Sebastien Lepers seb@meliweb.fr

RUN apt-get update

# Magento needed PHP exts
RUN requirements="libpng12-dev libmcrypt-dev libmcrypt4 libcurl3-dev libfreetype6 libjpeg62-turbo libpng12-dev libfreetype6-dev libjpeg62-turbo-dev libxml2-dev" \
    && apt-get install -y $requirements \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install soap \
    && requirementsToRemove="libpng12-dev libmcrypt-dev libcurl3-dev libpng12-dev libfreetype6-dev libjpeg62-turbo-dev libxml2-dev" \
    && apt-get purge --auto-remove -y $requirementsToRemove

RUN usermod -u 1000 www-data

COPY php.ini.sample /usr/local/etc/php/php.ini

WORKDIR /var/www/htdocs

# SMTP Mailcatcher
RUN apt-get install -y ssmtp
RUN sed -i "s/mailhub=mail/mailhub=my_smtp/" /etc/ssmtp/ssmtp.conf

# Composer
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# Git (needed by Composer)
RUN apt-get install -y git

# MySQL cli
RUN apt-get install -y mysql-client

# Magerun
ENV N98_MAGERUN_URL http://files.magerun.net/n98-magerun-latest.phar

RUN curl -o /usr/local/bin/magerun $N98_MAGERUN_URL \
    && chmod +x /usr/local/bin/magerun

# Clean
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

USER www-data

# Keep running with compose
CMD ["sleep", "infinity"]
