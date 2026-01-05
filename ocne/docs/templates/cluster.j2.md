# cluster.j2 Template Documentation

## Purpose

Defines a Kubernetes `CephCluster` custom resource for use with Rook, configuring a full Ceph storage cluster for use as a backend for persistent storage in the environment.

## How the Template is Used

- Rendered by ceph_deployments.yml and distributed to every control plane node.
- Consumed by the `rook-ceph-operator` or any workflow needing to instantiate a Ceph cluster via `kubectl apply -f cluster.yaml`.
- Forms the foundation for all block/persistent storage within the K8s infrastructure.

## Key Variables & Substitutions

- This template has no Jinja2 substitutions; it is rendered as a static YAML manifest.

## Rendered Example

```yaml
apiVersion: ceph.rook.io/v1
kind: CephCluster
metadata:
  name: rook-ceph
  namespace: rook
spec:
  cephVersion:
    image: container-registry.oracle.com/olcne/ceph:v17.2.5
    imagePullPolicy: Always
    allowUnsupported: false
  dataDirHostPath: /var/lib/rook
  mon:
    count: 3
    allowMultiplePerNode: false
  dashboard:
    enabled: false
  storage:
    useAllNodes: true
    useAllDevices: false
    deviceFilter: sdb
```

## Special Logic

- No loops or conditionals; intended to be copied verbatim to the cluster.
- Default device assignment is to `/dev/sdb`, using all nodes for storage.

## Related Files

- Rendered by: ceph_deployments.yml (see its documentation).
- Consumed by: Rook/Ceph operator in the cluster.
- Closely associated with toolbox.j2, filesystem.j2, storageclass.j2, pvc.j2, vm.j2 as part of full storage stack.
