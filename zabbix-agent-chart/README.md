# Zabbix agent

[Zabbix](https://www.zabbix.com/) is a monitoring system.

Zabbix agent is used by zabbix server to run checks.

## TL;DR;

```console
$ helm repo add fermosit https://harbor.fermosit.es/chartrepo/library
$ helm upgrade --install fermosit/zabbix-agent
```

## Introduction

This chart bootstraps a [Zabbix agent](https://www.zabbix.com/documentation/5.4/manual/installation/containers) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Source code:  https://github.com/elmanytas/zabbix-kubernetes/zabbix-agent

Report bugs here: https://github.com/elmanytas/zabbix-kubernetes/issues

## Prerequisites

- Kubernetes 1.12+
- Helm 2.11+ or Helm 3.0-beta4+

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm repo add fermosit https://harbor.fermosit.es/chartrepo/library
$ helm install my-release fermosit/zabbix-agent
```

The command deploys Zabbix Agent on the Kubernetes cluster using default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the Zabbix Agent chart and their default values.

|            Parameter                 |                    Description                                 |                           Default                       |
| ------------------------------------ | -------------------------------------------------------------- | ------------------------------------------------------- |
| `image.registry`                     | Zabbix agent image registry                                    | `docker.io`                                             |
| `image.repository`                   | Zabbix agent image name                                        | `zabbix/zabbix-agent`                                   |
| `image.tag`                          | Zabbix agent image tag                                         | `{TAG_NAME}`                                            |
| `image.pullPolicy`                   | Image pull policy                                              | `IfNotPresent`                                          |
| `image.pullSecrets`                  | Specify docker-registry secret names as an array               | `[]` (does not add image pull secrets to deployed pods) |
| `nameOverride.`                      | Strings to partially override fullname templates with a string (will prepend the release name) | `nil`                   |
| `fullnameOverride.`                  | Strings to fully override fullname templates with a string     | `nil`                                                   |
| `livenessProbe.initialDelaySeconds`  | Delay before liveness probe is initiated                       | `60`                                                    |
| `livenessProbe.periodSeconds`        | How often to perform the probe                                 | `10`                                                    |
| `livenessProbe.timeoutSeconds`       | When the probe times out                                       | `5`                                                     |
| `livenessProbe.failureThreshold`     | Minimum consecutive failures for the probe                     | `6`                                                     |
| `livenessProbe.successThreshold`     | Minimum consecutive successes for the probe                    | `1`                                                     |
| `readinessProbe.initialDelaySeconds` | Delay before readiness probe is initiated                      | `10`                                                    |
| `readinessProbe.periodSeconds`       | How often to perform the probe                                 | `10`                                                    |
| `readinessProbe.timeoutSeconds`      | When the probe times out                                       | `5`                                                     |
| `readinessProbe.failureThreshold`    | Minimum consecutive failures for the probe                     | `6`                                                     |
| `readinessProbe.successThreshold`    | Minimum consecutive successes for the probe                    | `1`                                                     |
| `schedulerName`                      | Name of the alternate scheduler                                | `nil`                                                   |
| `nodeSelector`                       | Node labels for pod assignment                                 | `{}`                                                    |
| `tolerations`                        | List of node taints to tolerate                                | `[]`                                                    |
| `affinity`                           | Map of node/pod affinities                                     | `{}`                                                    |
| `podAnnotations`                     | Pod annotations                                                | `{}`                                                    |
| `updateStrategy`                     | Set up update strategy                                         | `RollingUpdate` for web and `Recreate` for server       |
| `zabbix_vars`                        | Variables in lowercase passed to deployments in uppercase      | Default zabbix variables                                |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
helm repo add fermosit https://harbor.fermosit.es/chartrepo/library
helm upgrade --install zabbix-agent fermosit/zabbix-agent \
  --namespace zabbix \
  --set zabbix_vars.zbx_activeservers=zabbix.example.com \
  --set zabbix_vars.zbx_hostname=agent.example.com
```

The above command sets the visible Zabbix installation name in right top corner of the web interface.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml fermosit/zabbix-agent
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### Zabbix variables

Zabbix variables are defined in `zabbix_vars` dictionary in lowercase.

All variables are converted to uppercase in deployment time.

Take a look to the [repository readme](https://hub.docker.com/r/zabbix/zabbix-agent) to configure all variables you need.

### Active/pasive checks

Zabbix supports active and passive checks.

Pasive checks configuration example:
```
zabbix_vars:
  zbx_passiveservers=zabbix.example.com
```

Active checks configuration example:
```
zabbix_vars:
  zbx_activeservers=zabbix.example.com
  zbx_hostname=agent_name
```

Remember that zabbix uses ports 10050 and 10051 for passive and active checks, so, if agent and server are not in the same kubernetes cluster,
you need to expose the right port using a LoadBalancer service in agent or server or add in your nginx ingress controller something similar to:
```
tcp:
  '10050': zabbix/zabbix-zabbix-agent:10050
```

In a Kubernetes environment using active checks could be easyer than using pasive checks.