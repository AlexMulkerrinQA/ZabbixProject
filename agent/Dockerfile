FROM ubuntu:trusty
MAINTAINER Alex Mulkerrin <Alexander.Mulkerrin@qa.com>

# make sure to get most recent repository location
RUN apt-get update
RUN apt-get install -y wget
RUN wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+trusty_all.deb
RUN dpkg -i zabbix-release_3.2-1+trusty_all.deb
RUN apt-get update

RUN apt-get install -y zabbix-agent

EXPOSE 10050
ENTRYPOINT service zabbix-agent start && bash