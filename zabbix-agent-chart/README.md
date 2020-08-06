# Zabbix agent

[Zabbix](https://www.zabbix.com/) is a monitoring system.

Zabbix agent

## TL;DR;

```console
$ helm upgrade --install zabbix-agent .
```

## Introduction

This chart bootstraps a [Zabbix agent](https://www.zabbix.com/documentation/4.4/manual/installation/containers) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm upgrade --install my-release .
```

The command deploys Zabbix on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm uninstall my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the WordPress chart and their default values.

|            Parameter                       |                                  Description                                 |                           Default                            |
| ------------------------------------------ | ---------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `image.zabbix_agent.registry`              | Zabbix agent image registry                                                  | `docker.io`                                                  |
| `image.zabbix_agent.repository`            | Zabbix agent image name                                                      | `zabbix/zabbix-agent`                                        |
| `image.zabbix_agent.tag`                   | Zabbix agent image tag                                                       | `{TAG_NAME}`                                                 |
| `image.zabbix_agent.pullPolicy`            | Image pull policy                                                            | `IfNotPresent`                                               |
| `image.zabbix_agent.pullSecrets`           | Specify docker-registry secret names as an array                             | `[]` (does not add image pull secrets to deployed pods)      |
| `nameOverride.`                            | Strings to partially override fullname templates with a string (will prepend the release name) | `nil`                                      |
| `fullnameOverride.`                        | Strings to fully override fullname templates with a string                                     | `nil`                                      |
| `schedulerName`                           | Name of the alternate scheduler                                               | `nil`                                                        |
| `nodeSelector`                            | Node labels for pod assignment                                                | `{}`                                                         |
| `tolerations`                             | List of node taints to tolerate                                               | `[]`                                                         |
| `affinity`                                | Map of node/pod affinities                                                    | `{}`                                                         |
| `podAnnotations`                          | Pod annotations                                                               | `{}`                                                         |
| `updateStrategy`                          | Set up update strategy                                                        | `RollingUpdate` for pasive monitoring and `Recreate` for active monitoring         |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm upgrade --install my-release \
    --set zabbix_vars.zbx_server_name=zabbix \
    .
```

The above command sets the visible Zabbix installation name in right top corner of the web interface.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm upgrade --install my-release -f values.yaml .
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### Active vs pasive monitorization

Using pasive monitorization will enable us to have multiple replicas because zabbix agent receives requests from zabbix server.

Using active monitorization force us to have only one replica because zabbix agent sends requests to zabbix server and you don't want duplicated data.
