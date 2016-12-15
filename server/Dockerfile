FROM ubuntu:trusty
MAINTAINER Alex Mulkerrin <Alexander.Mulkerrin@qa.com>

# make sure to get most recent repository location
RUN apt-get update
RUN apt-get install -y wget
RUN wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+trusty_all.deb
RUN dpkg -i zabbix-release_3.2-1+trusty_all.deb
RUN apt-get update

# download server and frontend packages
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y zabbix-server-mysql zabbix-frontend-php

# make default zabbix database tables
RUN service mysql start\
&&	mysql -u root -e "create database zabbix; grant all privileges on zabbix.* to zabbix@localhost identified by '';# flush privileges;" \
&& zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix zabbix

# php configuration
RUN sed -i 's/# php_value date.timezone .*/php_value date.timezone Europe\/London/' /etc/zabbix/apache.conf

# zabbix configuration
RUN cp /usr/share/zabbix/conf/zabbix.conf.php.example /etc/zabbix/web/zabbix.conf.php

EXPOSE 80/TCP 443/TCP
ENTRYPOINT service mysql start && service apache2 start && service zabbix-server start && bash