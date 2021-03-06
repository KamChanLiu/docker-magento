FROM centos:7

MAINTAINER Kam Liu <kamchanliu@gmail.com>

# Add the ngix repository
COPY config/nginx/nginx.repo /etc/yum.repos.d/nginx.repo

# Install EPEL and Remi
RUN yum -y install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

# Enable Remi PHP 7.0
RUN yum-config-manager --enable remi-php70

# Install Supervisor, NginX, PHP and Extensions
RUN yum -y install  nginx supervisor \
                    php \
                    php-bcmath \
                    php-cli \
                    php-fpm \
                    php-gd \
                    php-intl \
                    php-mbstring \
                    php-mcrypt \
                    php-mysqlnd \
                    php-pdo \
                    php-pecl-igbinary \
                    php-pecl-imagick \
                    php-pecl-oauth \
                    php-pecl-redis \
                    php-pecl-zendopcache \
                    php-process \
                    php-soap \
                    php-xml \
                    php-zip && yum clean all

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

# Add Web servers configurations
COPY config/nginx/nginx.conf /etc/nginx/nginx.conf
COPY config/nginx/magento.conf /etc/nginx/conf.d/default.conf

# Add PHP-FPM configuration
COPY config/php/www.conf /etc/php-fpm.d/www.conf

# Add Supervisor configuration
COPY config/supervisor/cronpy.ini /etc/supervisord.d/cronpy.ini
COPY config/supervisor/nginx.ini /etc/supervisord.d/nginx.ini.sample

# Add cronpy for Magento
COPY bin/cronpy /usr/local/bin/cronpy
RUN chmod -v +x /usr/local/bin/cronpy

# Install Python schedule library for cronpy to run
RUN curl https://bootstrap.pypa.io/get-pip.py | python && pip install schedule

# Add Kick-start script
COPY bin/kick-start /usr/local/bin/kick-start
RUN chmod -v +x /usr/local/bin/kick-start

# create mount point and chown it
RUN mkdir -p /var/www/html
RUN chown -R apache:apache /var/www/html

# create cache dirs and chown it
RUN mkdir -p /usr/share/httpd/.composer/cache
RUN chown -R apache:apache /usr/share/httpd/.composer/cache

# set permissions on php paths
RUN mkdir -p /var/lib/php/session/
RUN chown -R apache:apache /var/lib/php/

RUN mkdir -p /run/php-fpm/
RUN chown -R apache:apache /run/php-fpm/

EXPOSE 80

VOLUME ["/var/www/html", "/var/log/nginx", "/var/log/php-fpm"]

CMD ["kick-start"]
