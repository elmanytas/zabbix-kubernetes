FROM php:7.4-apache

RUN apt-get update \
 && apt-get -y install git \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN git clone https://bitbucket.org/sivann/zabbix_http_gateway.git \
    && mv zabbix_http_gateway/http2zabbix.php /var/www/html/

RUN sed -i "s/Listen 80/Listen 8080/g" /etc/apache2/ports.conf

USER 1000
EXPOSE 8080
