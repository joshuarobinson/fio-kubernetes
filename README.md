# fio-kubernetes

Example approaches for running parallel FIO tests in Kubernetes to test either RWO or RWX volumes.

For more detailed explanations please see the [accompanying blog post](https://medium.com/@joshua_robinson/storage-benchmarking-with-fio-in-kubernetes-14cf29dc5375).

Two different approaches are shown: 1) a simple deployment and PVC for RWX volumes and 2) a statefulset for RWO/RWX. Tradeoffs between the two approaches discussed below.

FIO job configs are stored as a configMap object in configs.yaml. There is currently one config, but additional configs could be added.

The docker image is a simple Alpine image based on FIO and is only ~10MB in size.

Deployment + PVC : The deployment makes scaling simple, with all pods connecting to the same dynamically provisioned persistentVolume and using a different output path to avoid collisions. This approach requires a single RWX volume, testing performance on a single shared filesystem.

Statefulset: each replica creates it's own volume, making this approach suitable for RWO volumes as well as RWX. Note that some shared filesystems will perform better if each node is using it's own filesystem, i.e., no true sharing.
