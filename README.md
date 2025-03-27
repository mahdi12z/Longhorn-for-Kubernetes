# Longhorn-for-Kubernetes
## Longhorn for Kubernetes - Overview & Deployment Guide

## What is Longhorn ?
longhorn is a lightweight, reliable, and powerful distributed block storage system built specifically for Kubernetes.
Developed by Rancher (SUSE), it enables highly available persistent volumes without relying on external storage systems like NFS or SAN.




# Key Features of Longhorn
## Replication & High Availability
Each volume can be replicated across multiple nodes. If a node fails, data remains available.

## Dynamic Provisioning
No need to create PVs manually. Just define a PVC and Longhorn handles the rest.

## ReadWriteMany (RWX)
Pods across multiple nodes can simultaneously read/write to the same volume using Longhorn's Share Manager.

## Snapshots & Backups
Take point-in-time snapshots or send backups to S3/NFS with ease.

## Disaster Recovery
Restore volumes on another node or cluster from backup in case of failure.

## User-Friendly Web UI
Web dashboard for managing volumes, replicas, nodes, snapshots, and backups.

## Monitoring Support
Full integration with Prometheus and Grafana for visibility and alerting.

## Volume Expansion
Resize volumes live without downtime.

## CSI Compatible
Fully supports Kubernetes native PVC, works with Helm charts, StatefulSets, and more.

## Deployment Guide
Prerequisites
Ensure each node has enough disk space (default path: /var/lib/longhorn).

Internet access is required for pulling container images.

DaemonSet for Longhorn Manager should run on all nodes.


# 1-Installation Steps 
1.Apply the Longhorn deployment manifest:
```bash
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.8.1/deploy/longhorn.yaml

```
2.Monitor installation progress:
```bash
kubectl get pods -n longhorn-system --watch
```

3.Verify that all components are running:
```bash
kubectl -n longhorn-system get pods

```
# 2- Enable UI Access via NodePort
Patch the service to expose Longhorn UI on NodePort:

```bash
kubectl patch svc longhorn-frontend -n longhorn-system -p '{"spec": {"type": "NodePort"}}'
```

Then open in your browser:

[http://<NodeIP>:<NodePort>](http://<NodeIP>:<NodePort>)

# 3-Set Longhorn as Default StorageClass
Ensure Longhorn is the default StorageClass by running:
```bash
kubectl patch storageclass longhorn -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## Installed Resources Overview
Namespace: longhorn-system
DaemonSet: longhorn-manager
Deployments: longhorn-ui, longhorn-driver-deployer
CRDs: volumes, replicas, engines, settings, backups, etc.
StorageClass: longhorn (default)
CSI components: provisioner, attacher, resizer, snapshotter


link :https://longhorn.io/docs/1.8.1/deploy/install/install-with-kubectl/
