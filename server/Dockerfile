FROM ubuntu:trusty
MAINTAINER Alex Mulkerrin <Alexander.Mulkerrin@qa.com>

# download server and frontend packages
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y zabbix-server-mysql zabbix-frontend-php

# make default zabbix database tables
WORKDIR /usr/share/zabbix-server-mysql/
RUN gunzip *.gz
RUN service mysql start\
&&	mysql -u root -e "create database zabbix; grant all privileges on zabbix.* to zabbix@localhost identified by ''; flush privileges;" \
&& cat schema.sql images.sql data.sql | mysql -u zabbix zabbix

# php configuration
WORKDIR /etc/php5/apache2/
RUN sed -i 's/post_max_size =.*/post_max_size = 16M/' php.ini
RUN sed -i 's/max_execution_time =.*/max_execution_time = 300/' php.ini
RUN sed -i 's/max_input_time =.*/max_input_time = 300/' php.ini
RUN sed -i 's/date.timezone =.*/date.timezone = UTC/' php.ini

# zabbix configuration
RUN cp /usr/share/doc/zabbix-frontend-php/examples/zabbix.conf.php.example /etc/zabbix/zabbix.conf.php
RUN sed -i '13s/.*/$DB["PASSWORD"] = "";/' /etc/zabbix/zabbix.conf.php
RUN cp /usr/share/doc/zabbix-frontend-php/examples/apache.conf /etc/apache2/conf-enabled/zabbix.conf


RUN sed -i 's/START=no/START=yes/' /etc/default/zabbix-server

EXPOSE 80/TCP 443/TCP
ENTRYPOINT service mysql start && service apache2 start && service zabbix-server start && bash