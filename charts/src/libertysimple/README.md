![Liberty logo](https://pbs.twimg.com/media/DFSr-oYXkAAuMWh.jpg)

# IBM WebSphere Application Server Liberty Profile


## Getting Help

More documentation can be found [here](https://developer.ibm.com/wasdev/websphere-liberty/).


# Introduction

This chart deploys a single Liberty Demo App instance into an IBM Cloud private or other Kubernetes environment.

## Prerequisites

- Kubernetes 1.7 or greater, with beta APIs enabled

## Installing the Chart

To install the chart with the release name `libertysimple`:

```sh
helm install --name libertysimple DEMO/libertysimple
```

> **Tip**: See all the resources deployed by the chart using `kubectl get all -l release=libertysimple`

## Uninstalling the Chart

To uninstall/delete the `libertysimple` release:

```sh
helm delete libertysimple
```

The command removes all the Kubernetes components associated with the chart, except any Persistent Volume Claims (PVCs).  This is the default behavior of Kubernetes, and ensures that valuable data is not deleted.  In order to delete the PVC use the following command:

```sh
kubectl delete pvc -l release=libertysimple
```
# Version  for Node Red
tag: 0.17.5

#Default Node Port
nodePort: 34242



# Copyright

© Copyright Niklaus Hirt 2017
