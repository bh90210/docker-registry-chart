<p align="center">
  <img width="26%" src="https://user-images.githubusercontent.com/22690219/129286274-7ccbe256-b0ea-42d9-83b0-9f047b2386a3.png" />
</p>

[![Release Charts](https://github.com/bh90210/docker-registry-chart/actions/workflows/release.yml/badge.svg)](https://github.com/bh90210/docker-registry-chart/actions/workflows/release.yml)


Forked from the [original charts repo](https://github.com/helm/charts/tree/master/stable/docker-registry).

# Docker Registry Helm Chart

This repo contains a Kubernetes chart to deploy a private Docker Registry.

## Quick Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

  	helm repo add docker-registry https://bh90210.github.io/docker-registry-chart

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
docker-registry` to see the charts.

To install the <chart-name> chart:

    helm install docker-registry docker-registry/docker-registry

To uninstall the chart:

    helm delete docker-registry

## Prerequisites Details

* PV support on underlying infrastructure (if persistence is required)

## Chart Details

This chart will do the following:

* Implement a Docker registry deployment

## Installing the Chart with custom values

To install the chart, use the following:

```
helm install docker-registry docker-registry/docker-registry -f values.yaml
```

## Configuration

The following table lists the configurable parameters of the docker-registry chart and
their default values.

| Parameter                   | Description                                                                                | Default         |
|:----------------------------|:-------------------------------------------------------------------------------------------|:----------------|
| `image.pullPolicy`          | Container pull policy                                                                      | `IfNotPresent`  |
| `image.repository`          | Container image to use                                                                     | `registry`      |
| `image.tag`                 | Container image tag to deploy                                                              | `2.7.1`         |
| `imagePullSecrets`          | Specify image pull secrets                                                                 | `nil` (does not add image pull secrets to deployed pods) |
| `persistence.accessMode`    | Access mode to use for PVC                                                                 | `ReadWriteOnce` |
| `persistence.enabled`       | Whether to use a PVC for the Docker storage                                                | `false`         |
| `persistence.deleteEnabled` | Enable the deletion of image blobs and manifests by digest                                 | `nil`           |
| `persistence.size`          | Amount of space to claim for PVC                                                           | `10Gi`          |
| `persistence.storageClass`  | Storage Class to use for PVC                                                               | `-`             |
| `persistence.existingClaim` | Name of an existing PVC to use for config                                                  | `nil`           |
| `service.port`              | TCP port on which the service is exposed                                                   | `5000`          |
| `service.type`              | service type                                                                               | `ClusterIP`     |
| `service.clusterIP`         | if `service.type` is `ClusterIP` and this is non-empty, sets the cluster IP of the service | `nil`           |
| `service.nodePort`          | if `service.type` is `NodePort` and this is non-empty, sets the node port of the service   | `nil`           |
| `service.loadBalancerIP`     | if `service.type` is `LoadBalancer` and this is non-empty, sets the loadBalancerIP of the service | `nil`          |
| `service.loadBalancerSourceRanges`| if `service.type` is `LoadBalancer` and this is non-empty, sets the loadBalancerSourceRanges of the service | `nil`           |
| `replicaCount`              | k8s replicas                                                                               | `1`             |
| `updateStrategy`            | update strategy for deployment                                                             | `{}`            |
| `podAnnotations`            | Annotations for pod                                                                        | `{}`            |
| `podLabels`                 | Labels for pod                                                                             | `{}`            |
| `podDisruptionBudget`       | Pod disruption budget                                                                      | `{}`            |
| `resources.limits.cpu`      | Container requested CPU                                                                    | `nil`           |
| `resources.limits.memory`   | Container requested memory                                                                 | `nil`           |
| `priorityClassName      `   | priorityClassName                                                                          | `""`            |
| `storage`                   | Storage system to use                                                                      | `filesystem`    |
| `tlsSecretName`             | Name of secret for TLS certs                                                               | `nil`           |
| `secrets.htpasswd`          | Htpasswd authentication                                                                    | `nil`           |
| `secrets.s3.accessKey`      | Access Key for S3 configuration                                                            | `nil`           |
| `secrets.s3.secretKey`      | Secret Key for S3 configuration                                                            | `nil`           |
| `secrets.swift.username`    | Username for Swift configuration                                                           | `nil`           |
| `secrets.swift.password`    | Password for Swift configuration                                                           | `nil`           |
| `haSharedSecret`            | Shared secret for Registry                                                                 | `nil`           |
| `configData`                | Configuration hash for docker                                                              | `nil`           |
| `s3.region`                 | S3 region                                                                                  | `nil`           |
| `s3.regionEndpoint`         | S3 region endpoint                                                                         | `nil`           |
| `s3.bucket`                 | S3 bucket name                                                                             | `nil`           |
| `s3.encrypt`                | Store images in encrypted format                                                           | `nil`           |
| `s3.secure`                 | Use HTTPS                                                                                  | `nil`           |
| `swift.authurl`             | Swift authurl                                                                              | `nil`           |
| `swift.container`           | Swift container                                                                            | `nil`           |
| `nodeSelector`              | node labels for pod assignment                                                             | `{}`            |
| `affinity`                  | affinity settings                                                                          | `{}`            |
| `tolerations`               | pod tolerations                                                                            | `[]`            |
| `ingress.enabled`           | If true, Ingress will be created                                                           | `false`         |
| `ingress.annotations`       | Ingress annotations                                                                        | `{}`            |
| `ingress.labels`            | Ingress labels                                                                             | `{}`            |
| `ingress.path`              | Ingress service path                                                                       | `/`             |
| `ingress.hosts`             | Ingress hostnames                                                                          | `[]`            |
| `ingress.tls`               | Ingress TLS configuration (YAML)                                                           | `[]`            |
| `extraVolumeMounts`         | Additional volumeMounts to the registry container                                          | `[]`            |
| `extraVolumes`              | Additional volumes to the pod                                                              | `[]`            |

Specify each parameter using the `--set key=value[,key=value]` argument to
`helm install`.

To generate htpasswd file, run this docker command:
`docker run --entrypoint htpasswd registry:2 -Bbn user password > ./htpasswd`.
