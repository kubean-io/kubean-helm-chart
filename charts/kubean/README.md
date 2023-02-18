# :seedling: kubean

[![helm workflow](https://github.com/kubean-io/kubean-helm-chart/actions/workflows/helm-release.yaml/badge.svg)](https://github.com/kubean-io/kubean-helm-chart/actions/workflows/helm-release.yaml)

## Introduction

kubean is a cluster lifecycle management tool based on [kubespray](https://github.com/kubernetes-sigs/kubespray).

## Features

The Kubean provides the following features.

* Based on the CRD cluster deployment method, all operations can be completed on the kubernetes API-server.

* Supports concurrent deployment of multiple clusters at the same time.

* Support air gap installation.(experimental)

* Support for both AMD64 and ARM64.

## Install

First, add the Kubean chart repo to your local repository.
``` bash 
$ helm repo add kubean-io https://kubean-io.github.io/kubean-helm-chart/

$ helm repo list
NAME          	URL
kubean-io     	https://kubean-io.github.io/kubean-helm-chart/
```

With the repo added, available charts and versions can be viewed.
``` bash
$ helm search repo kubean
```

You can run the following command to install kubean.
``` bash
$ helm install kubean kubean-io/kubean --create-namespace -n kubean-system
```

View cluster information.
``` bash
$ kubectl get clusters.kubean.io
```

View cluster operation jobs.
``` bash
$ kubectl get clusteroperations.kubean.io
```

## Uninstall

If kubean's related custom resources already exist, you need to clear.
``` bash
$ kubectl delete clusteroperations.kubean.io --all
$ kubectl delete clusters.kubean.io --all
$ kubectl delete manifests.kubean.io --all
$ kubectl delete localartifactsets.kubean.io --all
```

Uninstall kubean's components via helm.
``` bash
$ helm -n kubean-system uninstall kubean
$ kubectl delete crd clusteroperations.kubean.io
$ kubectl delete crd clusters.kubean.io
$ kubectl delete crd manifests.kubean.io
$ kubectl delete crd localartifactsets.kubean.io
```

## Parameters
**kubeanOperator parameters**

| Name	                       | Description	                                                         | Value                       | 
|-----------------------------|----------------------------------------------------------------------|-----------------------------|
| replicaCount	               | Number of replicas	                                                  | 1	                          |
| nameOverride                | Override the name of the chart used to generate resource names	      | ""	                         |
| fullnameOverride            | Override the full name of the chart used to generate resource names	 | ""	                         |
| podAnnotations              | Annotations to be added to the pod	                                  | {}	                         |
| podSecurityContext          | Security context for the pod	                                        | {}	                         |
| securityContext	            | Security context for the container	                                  | {}	                         |
| serviceAccount.create	      | Create a service account	                                            | true                        |
| serviceAccount.annotations	 | Annotations to be added to the service account	                      | {}                          |
| serviceAccount.name	        | Name of the service account to use	                                  | ""                          |
| image.registry	             | Image registry	                                                      | "ghcr.io"                   |
| image.repository	           | Image repository	                                                    | "kubean-io/kubean-operator" |
| image.tag	                  | Image tag	                                                           | "v0.4.4-rc1"                |
| image.pullPolicy	           | Image pull policy	                                                   | "IfNotPresent"              |
| imagePullSecrets	           | Image pull secrets	                                                  | []                          |
| service.type	               | Service type	                                                        | "ClusterIP"                 |
| service.port	               | Service port	                                                        | 80                          |
| resources	                  | limits and request setting	                                          | {}                          |
| nodeSelector	               | Node selector	                                                       | {}                          |
| tolerations	                | Tolerations	                                                         | []                          |
| configMap.BACKEND_LIMIT     | The maximum number of concurrent backend requests.                   | 10                          |

**sprayJob parameters**

| Name	             | Description	      | Value                 |
|-------------------|-------------------|-----------------------|
| image.registry	   | Image registry	   | "ghcr.io"             |
| image.repository	 | Image repository	 | "kubean-io/spray-job" |
| image.tag	        | Image tag	        | "v0.4.4-rc1"          |
