FROM debian:10

RUN apt-get update \
 && apt-get -y install git python \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN git clone https://bitbucket.org/sivann/zabbix_http_gateway.git

CMD ["python", "zabbix_http_gateway/zabbix2http.py"]

USER 1000

EXPOSE 10051
