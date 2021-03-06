FROM akeneo/php:7.1
MAINTAINER Damien Carcel <damien.carcel@akeneo.com>

# Install Apache + mod_php and some PHP extensions
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests --yes --quiet install apache2 libapache2-mod-php7.1 && \
    apt-get clean && apt-get --yes --quiet autoremove --purge && \
    rm -rf  /var/lib/apt/lists/* /tmp/* /var/tmp/* \
            /usr/share/doc/* /usr/share/groff/* /usr/share/info/* /usr/share/linda/* \
            /usr/share/lintian/* /usr/share/locale/* /usr/share/man/*

# Configure PHP Apache
RUN phpenmod akeneo
RUN phpdismod xdebug

# Configure Apache
RUN a2enmod rewrite && \
    echo "ServerName localhost" >> /etc/apache2/apache2.conf

RUN sed -i "s/export APACHE_RUN_USER=www-data/export APACHE_RUN_USER=docker/" /etc/apache2/envvars && \
    sed -i "s/export APACHE_RUN_GROUP=www-data/export APACHE_RUN_GROUP=docker/" /etc/apache2/envvars && \
    chown -R docker: /var/lock/apache2

# Expose Apache
EXPOSE 80 443

COPY files/apache-foreground /usr/local/bin
RUN  chmod +x /usr/local/bin/apache-foreground

# Run apache in foreground
CMD ["sudo", "/usr/local/bin/apache-foreground"]
