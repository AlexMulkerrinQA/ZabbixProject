FROM ubuntu:trusty
MAINTAINER Alex Mulkerrin <Alexander.Mulkerrin@qa.com>

# download server and frontend packages
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install zabbix-server-mysql zabbix-frontend-php

# make default zabbix database tables
RUN service mysql service start
RUN mysql -u root -e "create database zabbix; grant all privileges on zabbix.* to zabbix@localhost identified by '';"
RUN zcat /usr/share/zabbix-server-mysql/*.gz | mysql -u zabbix zabbix

# php configuration
WORKDIR /etc/php5/apache2/
RUN sed -i 's/post_max_size =.*/post_max_size = 16M/' php.ini
RUN sed -i 's/max_execution_time =.*/max_execution_time = 300/' php.ini
RUN sed -i 's/max_input_time =.*/max_input_time = 300/' php.ini
RUN sed -i 's/date.timezone =.*/date.timezone = UTC/' php.ini

# zabbix configuration
RUN sed -i 's/START=no/START=yes/' /etc/default/zabbix-server

EXPOSE 80/TCP 443/TCP
ENTRYPOINT service mysql start && service apache2 start && service zabbix-server start && bash