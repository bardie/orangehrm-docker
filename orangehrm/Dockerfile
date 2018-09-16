FROM php:7.3-rc-apache
RUN apt-get update -y
ENV MYSQL_PWD root
RUN echo "mysql-server mysql-server/root_password password $MYSQL_PWD" | debconf-set-selections
RUN echo "mysql-server mysql-server/root_password_again password $MYSQL_PWD" | debconf-set-selections
WORKDIR /tmp
RUN apt-get install -y lsb-release wget gnupg software-properties-common
RUN apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
RUN add-apt-repository 'deb [arch=amd64] http://www.ftp.saix.net/DB/mariadb/repo/10.2/debian stretch main'
RUN apt-get update -y
RUN curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.3-1_all.deb
RUN dpkg -i mysql-apt-config*
RUN apt-get update -y
RUN apt-get -y install git unzip mariadb-server
COPY orangehrm.zip /var/www/
COPY php.ini /usr/local/etc/php
WORKDIR /var/www/
RUN unzip orangehrm.zip
RUN rm -rf /var/www/html
RUN mv orangehrm-4.1 html/
RUN chown www-data:www-data html/*
RUN docker-php-ext-install mysqli pdo pdo_mysql
COPY my.cnf /etc/mysql/my.cnf
RUN chown mysql:mysql /etc/mysql/my.cnf
COPY start /
RUN chmod u+x /start

CMD /start
