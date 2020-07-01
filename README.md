# zabbix for kubernetes

Zabbix is a monitoring system that allows to do traditional checks, protocol checks and functional application checks easily.

## helm chart

Zabbix images are available and provided by zabbix but there is not any official chart that deploys them in kubernetes.

You can use the chart in zabbix-mysql to deploy it.

## access between clusters with proxy

Other problem is to use Zabbix in a Kubernetes cluster to monitorice using an agent in other cluster because zabbix use its own protocol in tcp ports 10050 (pasive) and 10051 (active).

Standard ingress only provides http and https so I can use this code:
https://bitbucket.org/sivann/zabbix_http_gateway/src/master/

It allows using a http proxy.


