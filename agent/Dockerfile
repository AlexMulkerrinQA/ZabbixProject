FROM ubuntu:trusty
MAINTAINER Alex Mulkerrin <Alexander.Mulkerrin@qa.com>

# make sure to get most recent repository location
RUN apt-get update
RUN apt-get install -y wget
RUN wget http://repo.zabbix.com/zabbix/3.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.2-1+trusty_all.deb
RUN dpkg -i zabbix-release_3.2-1+trusty_all.deb
RUN apt-get update

RUN apt-get install -y zabbix-agent

WORKDIR /etc/zabbix/
#RUN SERVER=$(getent hosts zabbix-server | awk '{ print $1 '}) \
RUN SERVER="172.18.0.2" \
&& echo "server is $SERVER" \
&& sed -i "s/Server=127.0.0.1/Server=$SERVER/" zabbix_agentd.conf \
&& sed -i "s/ServerActive=127.0.0.1/ServerActive=$SERVER/" zabbix_agentd.conf
RUN sed -i 's/Hostname=Zabbix server/Hostname=zabbix-agent/' zabbix_agentd.conf

EXPOSE 10050
ENTRYPOINT service zabbix-agent start && bash
