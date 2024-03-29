# Zabbix mysql

[Zabbix](https://www.zabbix.com/) is a monitoring system.

Zabbix can use postgresql or mysql. This chart implementes the mysql flavor of zabbix.

## WARNING!!!

The version 3.0.0 breaks zabbix storage (if you use persistence.thing.enabled=true).

In previous versions a volume by directory was created. Now only is created one volume and each directory in zabbix is a directory in the volume so you need to move all data from each volume to each directory in the new volume.

## TL;DR;

```console
$ helm repo add fermosit https://harbor.fermosit.es/chartrepo/library
$ helm upgrade --install fermosit/zabbix-server-mysql
```

## Introduction

This chart bootstraps a [Zabbix](https://www.zabbix.com/documentation/4.4/manual/installation/containers) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

It also uses the [Bitnami MariaDB chart](https://github.com/kubernetes/charts/tree/master/stable/mariadb) which is required for bootstrapping a MariaDB deployment for the database requirements of the Zabbix application.

Source code:  https://github.com/elmanytas/zabbix-kubernetes/zabbix-server-mysql-chart

Report bugs here: https://github.com/elmanytas/zabbix-kubernetes/issues

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm repo add fermosit https://harbor.fermosit.es/chartrepo/library
$ helm install my-release fermosit/zabbix-server-mysql
```

The command deploys Zabbix on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete zabbix
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the Zabbix Server chart and their default values.

|            Parameter                       |                                  Description                                 |                           Default                            |
| ------------------------------------------ | ---------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `image.zabbix_server_mysql.registry`       | Zabbix server mysql image registry                                           | `docker.io`                                                  |
| `image.zabbix_server_mysql.repository`     | Zabbix server image name                                                     | `zabbix/zabbix-server-mysql`                                 |
| `image.zabbix_server_mysql.tag`            | Zabbix server image tag                                                      | `{TAG_NAME}`                                                 |
| `image.zabbix_server_mysql.pullPolicy`     | Image pull policy                                                            | `IfNotPresent`                                               |
| `image.zabbix_server_mysql.pullSecrets`    | Specify docker-registry secret names as an array                             | `[]` (does not add image pull secrets to deployed pods)      |
| `image.zabbix_web_nginx_mysql.registry`    | Zabbix web mysql image registry                                              | `docker.io`                                                  |
| `image.zabbix_web_nginx_mysql.repository`  | Zabbix web image name                                                        | `zabbix/zabbix-web-nginx-mysql`                              |
| `image.zabbix_web_nginx_mysql.tag`         | Zabbix web image tag                                                         | `{TAG_NAME}`                                                 |
| `image.zabbix_web_nginx_mysql.pullPolicy`  | Image pull policy                                                            | `IfNotPresent`                                               |
| `image.zabbix_web_nginx_mysql.pullSecrets` | Specify docker-registry secret names as an array                             | `[]` (does not add image pull secrets to deployed pods)      |
| `image.zabbix_agent.registry`              | Zabbix agent image registry                                                  | `docker.io`                                                  |
| `image.zabbix_agent.repository`            | Zabbix agent image name                                                      | `zabbix/zabbix-agent`                                        |
| `image.zabbix_agent.tag`                   | Zabbix agent image tag                                                       | `{TAG_NAME}`                                                 |
| `image.zabbix_agent.pullPolicy`            | Image pull policy                                                            | `IfNotPresent`                                               |
| `image.zabbix_agent.pullSecrets`           | Specify docker-registry secret names as an array                             | `[]` (does not add image pull secrets to deployed pods)      |
| `nameOverride.`                            | Strings to partially override fullname templates with a string (will prepend the release name) | `nil`                                      |
| `fullnameOverride.`                        | Strings to fully override fullname templates with a string                                     | `nil`                                      |
| `replicaCount.zabbix_web_nginx_mysql`      | Number of Web Pods to run                                                    | `2`                                                          |
| `service.zabbix_server_mysql.type`         | Kubernetes Service type                                                      | `ClusterIP`                                                  |
| `service.zabbix_server_mysql.port`         | Service TCP port                                                             | `10051`                                                      |
| `service.zabbix_web_nginx_mysql.type`      | Kubernetes Service type                                                      | `ClusterIP`                                                  |
| `service.zabbix_web_nginx_mysql.port`      | Service HTTP port                                                            | `80`                                                         |
| `zabbix_web_nginx.livenessProbe.initialDelaySeconds`  | Delay before liveness probe is initiated                          | `20`                                                         |
| `zabbix_web_nginx.livenessProbe.periodSeconds`        | How often to perform the probe                                    | `10`                                                         |
| `zabbix_web_nginx.livenessProbe.timeoutSeconds`       | When the probe times out                                          | `5`                                                          |
| `zabbix_web_nginx.livenessProbe.failureThreshold`     | Minimum consecutive failures for the probe                        | `6`                                                          |
| `zabbix_web_nginx.livenessProbe.successThreshold`     | Minimum consecutive successes for the probe                       | `1`                                                          |
| `zabbix_web_nginx.readinessProbe.initialDelaySeconds` | Delay before readiness probe is initiated                         | `10`                                                         |
| `zabbix_web_nginx.readinessProbe.periodSeconds`       | How often to perform the probe                                    | `10`                                                         |
| `zabbix_web_nginx.readinessProbe.timeoutSeconds`      | When the probe times out                                          | `5`                                                          |
| `zabbix_web_nginx.readinessProbe.failureThreshold`    | Minimum consecutive failures for the probe                        | `6`                                                          |
| `zabbix_web_nginx.readinessProbe.successThreshold`    | Minimum consecutive successes for the probe                       | `1`                                                          |
| `zabbix_server_mysql.livenessProbe.initialDelaySeconds`  | Delay before liveness probe is initiated                       | `60`                                                         |
| `zabbix_server_mysql.livenessProbe.periodSeconds`        | How often to perform the probe                                 | `10`                                                         |
| `zabbix_server_mysql.livenessProbe.timeoutSeconds`       | When the probe times out                                       | `5`                                                          |
| `zabbix_server_mysql.livenessProbe.failureThreshold`     | Minimum consecutive failures for the probe                     | `6`                                                          |
| `zabbix_server_mysql.livenessProbe.successThreshold`     | Minimum consecutive successes for the probe                    | `1`                                                          |
| `zabbix_server_mysql.readinessProbe.initialDelaySeconds` | Delay before readiness probe is initiated                      | `10`                                                         |
| `zabbix_server_mysql.readinessProbe.periodSeconds`       | How often to perform the probe                                 | `10`                                                         |
| `zabbix_server_mysql.readinessProbe.timeoutSeconds`      | When the probe times out                                       | `5`                                                          |
| `zabbix_server_mysql.readinessProbe.failureThreshold`    | Minimum consecutive failures for the probe                     | `6`                                                          |
| `zabbix_server_mysql.readinessProbe.successThreshold`    | Minimum consecutive successes for the probe                    | `1`                                                          |
| `ingress.enabled`                         | Enable ingress controller resource                                            | `false`                                                      |
| `ingress.hostname`                        | Default host for the ingress resource                                         | `zabbix.local`                                               |
| `ingress.annotations`                     | Ingress annotations                                                           | `[]`                                                         |
| `ingress.tls[0].hosts[0]`                 | TLS hosts                                                                     | `zabbix.local`                                               |
| `ingress.tls[0].secretName`               | TLS Secret (certificates)                                                     | `zabbix.local-tls`                                           |
| `ingress.secrets[0].name`                 | TLS Secret Name                                                               | `nil`                                                        |
| `ingress.secrets[0].certificate`          | TLS Secret Certificate                                                        | `nil`                                                        |
| `ingress.secrets[0].key`                  | TLS Secret Key                                                                | `nil`                                                        |
| `schedulerName`                           | Name of the alternate scheduler                                               | `nil`                                                        |
| `persistence.storageClass`                | PVC Storage  Class                                                            | `nil` (uses alpha storage class annotation)                  |
| `persistence.enabled`                     | Enable persistence using PVC                                                  | `false`                                                      |
| `persistence.accessMode`                  | PVC Access Mode                                                               | `ReadWriteOnce`                                              |
| `persistence.size`                        | PVC Storage Request                                                           | `100Mi`                                                      |
| `persistence.claimName`                   | PVC Storage Name                                                              | `alertscripts`                                               |
| `nodeSelector`                            | Node labels for pod assignment                                                | `{}`                                                         |
| `tolerations`                             | List of node taints to tolerate                                               | `[]`                                                         |
| `affinity`                                | Map of node/pod affinities                                                    | `{}`                                                         |
| `initContainers`                          | List of init containers (take a look to examples in values.yaml)              | `[]`                                                         |
| `podAnnotations`                          | Pod annotations                                                               | `{}`                                                         |
| `updateStrategy`                          | Set up update strategy                                                        | `RollingUpdate` for web and `Recreate` for server            |
| `zabbix_vars`                             | Variables in lowercase passed to deployments in uppercase                     | Default zabbix variables                                     |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
helm repo add fermosit https://harbor.fermosit.es/chartrepo/library
helm upgrade --install zabbix fermosit/zabbix-server-mysql \
  --namespace zabbix \
  --set ingress.enabled=true \
  --set ingress.hostname=zabbix.example.org \
  --set ingress.annotations."cert-manager\.io/cluster-issuer=letsencrypt-prod" \
  --set ingress.tls[0].hosts[0]=zabbix.example.org \
  --set ingress.tls[0].secretName=zabbix-example-org-tls
```

The above command sets the visible Zabbix installation name in right top corner of the web interface.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml fermosit/zabbix-server-mysql
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### Zabbix variables

Zabbix variables are defined in `zabbix_vars` dictionary in lowercase.

All variables are converted to uppercase in deployment time.

Take a look to the [repository readme](https://hub.docker.com/r/zabbix/zabbix-server-mysql) to configure all variables you need.

### Production configuration

- Number of Zabbix web pods to run
```diff
- replicaCount:
-   zabbix_web_nginx_mysql: 1
+ replicaCount:
+   zabbix_web_nginx_mysql: 2
```

### Ingress

This chart provides support for ingress resources. If you have an ingress controller installed on your cluster, such as [nginx-ingress](https://kubeapps.com/charts/stable/nginx-ingress) or [traefik](https://kubeapps.com/charts/stable/traefik) you can utilize the ingress controller to serve your WordPress application.

To enable ingress integration, please set `ingress.enabled` to `true`

A tipical deployment with https could be done like this:
```
helm upgrade --install zabbix fermosit/zabbix-server-mysql \
server-mysql-chart \
  --namespace zabbix \
  --set ingress.enabled=true \
  --set ingress.hostname=zabbix.example.org  \
  --set ingress.annotations."cert-manager\.io/cluster-issuer=letsencrypt-prod" \
  --set ingress.tls[0].hosts[0]=zabbix.example.org \
  --set ingress.tls[0].secretName=zabbix-example-org-es-tls
```

### initContainers

Now you can add initContainers to do some tasks like fix permissions. Take a look to initContainers examples in `values.yaml` .