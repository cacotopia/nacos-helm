# nacos - an easy-to-use dynamic service discovery, configuration and service management platform for building cloud native applications.

[Nacos](https://nacos.io) is an easy-to-use platform designed for dynamic service discovery and configuration and service management. It helps you to build cloud native applications and microservices platform easily.

## Introduction

This chart bootstraps nacos statefulset on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.6+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release nacos
```

The command deploys nacos cluster on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

>tip:
>The default user is: nacos
>The default password is: nacos

### Uninstall

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

## Configuration

The following table lists the configurable parameters of the chart and their default values.

| Parameter                  | Description                         | Default                                |
| -----------------------    | ----------------------------------- | -------------------------------------- |
| `statefulset.enabled`      | use statefulset to start            | `true`                                 |
| `global`                   | Global setting                      | see in values.yaml                     |
| `deploymentStrategy`       | deployment rollingUpdate setting    | `{}`                                   |
| `replicaCount`             | replicas number                     | `3`                                    |
| `service`                  | Service type, protocol, port        | `ClusterIP` `TCP` 8848                 |
| `env`                      | container env setting               | `[]`                                   |
| `startCommand`             | Start command                       | `[]`                                   |
| `config`                   | Additional configmap to use         | see in `values.yaml`                   |
| `secret`                   | Additional secret to use            | see in `values.yaml`                   |
| `image`                    | `nacos` image, tag.                 | `nacos/nacos-server` `1.1.4`           |
| `ingress`                  | Ingress for the nacos.              | `false`                                |
| `persistentVolume.enabled` | Create a volume to store data       | `false`                                |
| `persistentVolume.storageClass` | Type of persistent volume claim| `nil`                                  |
| `persistentVolume.accessModes`  | Persistent volume access modes | `[ReadWriteOnce]`                      |
| `persistentVolume.size`         | Persistent volume access modes | `2Gi`                                  |
| `persistentVolume.existingClaim`| Persistent volume existingClaim name| `{}`                              |
| `persistentVolume.mountPaths`   | Persistent directory path      | see in `values.yaml`                   |
| `persistentVolume.annotations`  | Persistent volume annotations  | `{}`                                   |
| `healthCheck.enabled`      | liveness and readiness probes       | `true`                                 |
| `mysql`                    | mysql for nacos                     | see in values.yaml                     |
| `resources`                | CPU/Memory resource requests/limits | `{}`                                   |
| `lifecycle`                | Pod lifecycle                       | `{}`                                   |
| `deployment.additionalVolumes`| Deployment additionalVolumes     | `[]`                                   |
| `additionalContainers`     | sidecar containers                  | `{}`                                   |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

## Persistence

The [nacos image](https://github.com/nacos-group/nacos-docker) stores the data and configurations at the `/home/nacos/{plugins,data,logs}` path of the container.

## Nacos Common property configuration

| name                          | description                            | option                                 |
| ----------------------------- | -------------------------------------- | -------------------------------------- |
| `MODE`                          | cluster/standalone                     | cluster/standalone default **cluster** |
| `NACOS_SERVERS`                 | nacos cluster address        | eg. ip1:port1 ip2:port2 ip3:port3             |
| `PREFER_HOST_MODE`              | Whether hostname are supported         | hostname/ip default **ip**             |
| `NACOS_APPLICATION_PORT`             | nacos server port                      | default **8848**                       |
| `NACOS_SERVER_IP`             | custom nacos server ip when network was mutil-network                      |                         |
| `SPRING_DATASOURCE_PLATFORM`    | standalone support mysql               | mysql / empty default empty            |
| `MYSQL_SERVICE_HOST` | mysql  host |  |
| `MYSQL_SERVICE_PORT` | mysql  database port | default : **3306** |
| `MYSQL_SERVICE_DB_NAME` | mysql  database name |  |
| `MYSQL_SERVICE_USER` | username of  database |  |
| `MYSQL_SERVICE_PASSWORD` | password of  database |  |
| `MYSQL_DATABASE_NUM`      | It indicates the number of database             | default :**1**                      |
| `MYSQL_SERVICE_DB_PARAM`      | Database url parameter             | default : **characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useSSL=false**                      |
| `JVM_XMS`      |  -Xms             | default :1g                          |
| `JVM_XMX`      |  -Xmx            | default :1g                          |
| `JVM_XMN`      |  -Xmn           | default :512m                          |
| `JVM_MS`      |  -XX:MetaspaceSize          | default :128m                          |
| `JVM_MMS`      |  -XX:MaxMetaspaceSize          | default :320m                          |
| `NACOS_DEBUG`      |  enable remote debug          | y/n default :n                          |
| `TOMCAT_ACCESSLOG_ENABLED`      |  server.tomcat.accesslog.enabled         | default :false                          |
| `NACOS_AUTH_SYSTEM_TYPE`      |  The auth system to use, currently only 'nacos' is supported        | default :nacos                          |
| `NACOS_AUTH_ENABLE`      |  If turn on auth system        | default :false                          |
| `NACOS_AUTH_TOKEN_EXPIRE_SECONDS`      |  The token expiration in seconds        | default :18000                          |
| `NACOS_AUTH_TOKEN`      |  The default token        | default :SecretKey012345678901234567890123456789012345678901234567890123456789                          |
| `NACOS_AUTH_CACHE_ENABLE`      |  Turn on/off caching of auth information. By turning on this switch, the update of auth information would have a 15 seconds delay.        | default : false                          |
| `MEMBER_LIST`      |  Set the cluster list with a configuration file or command-line argument        | eg:192.168.16.101:8847?raft_port=8807,192.168.16.101?raft_port=8808,192.168.16.101:8849?raft_port=8809                          |
| `EMBEDDED_STORAGE`      |    Use embedded storage in cluster mode without mysql      | `embedded` default : none                          |
| `NACOS_AUTH_CACHE_ENABLE`      |    nacos.core.auth.caching.enabled      |  default : false                          |
| `NACOS_AUTH_USER_AGENT_AUTH_WHITE_ENABLE`      |    nacos.core.auth.enable.userAgentAuthWhite      |  default : false                          |
| `NACOS_AUTH_IDENTITY_KEY`      |    nacos.core.auth.server.identity.key      |  default : serverIdentity                          |
| `NACOS_AUTH_IDENTITY_VALUE`      |    nacos.core.auth.server.identity.value      |  default : security                          |
| `NACOS_SECURITY_IGNORE_URLS`      |    nacos.security.ignore.urls      |  default : `/,/error,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-fe/public/**,/v1/auth/**,/v1/console/health/**,/actuator/**,/v1/console/server/**`                          |
