# CSI-Flex beegfs driver

# Requirements

The folllowing feature gates and runtime config have to be enabled to deploy the driver

```
FEATURE_GATES=CSIPersistentVolume=true,MountPropagation=true
RUNTIME_CONFIG="storage.k8s.io/v1alpha1=true"
```

Mountprogpation requries support for privileged containers. So, make sure privileged containers are enabled in the cluster.

# Configuration

* must set BEEGFS_ROOT and beegfs-mount-dir to corresponding mount point for node local BeeGFS
* must set volumeHandle volumeAttributes.share appropriately

# Building

* must build the flexadapter first
* setup your values (registry etc.) in PrivateRules.mak
* make all
* adjust the resource descriptors for configuration and repos etc.

# Deploy in Kubernetes

```kubectl -f deploy/kubernetes create```

# Example Nginx application

``` kubectl -f examples/kubernetes/nginx.yaml create```
