# filesystem.j2 Template Documentation

## Purpose

Defines a Kubernetes `CephFilesystem` custom resource for Rook, setting up a distributed CephFS filesystem for use as a persistent, POSIX-compliant shared volume by the cluster.

## How the Template is Used

- Rendered by ceph_deployments.yml and deployed to each control plane node.
- Applied to the cluster by the Rook operator to create and manage CephFS storage pools, metadata servers, and provide multi-node file system access.

## Key Variables & Substitutions

- This template is fully static: no Jinja2 substitutions are present.

## Rendered Example

```yaml
apiVersion: ceph.rook.io/v1
kind: CephFilesystem
metadata:
  name: mycephfs
  namespace: rook
spec:
  metadataPool:
    replicated:
      size: 3
  dataPools:
    - name: replicated
      replicated:
        size: 3
  preserveFilesystemOnDelete: true
  metadataServer:
    activeCount: 1
    activeStandby: true
```

## Special Logic

- Replicated pools for both metadata and data, each with `size: 3` for durability.
- Single active metadata server with standby enabled.
- Filesystem is not deleted even when the CRD is removed (`preserveFilesystemOnDelete: true`).

## Related Files

- Rendered by: ceph_deployments.yml.
- Consumed by: Rook operator inside the K8s cluster.
- Used together with cluster.j2, storageclass.j2, pvc.j2, toolbox.j2, and vm.j2 for a full storage stack.
