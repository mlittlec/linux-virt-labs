# pvc.j2 Template Documentation

## Purpose

Defines a PersistentVolumeClaim for Kubernetes to dynamically provision and bind to a CephFS-backed storage volume using the rook-cephfs StorageClass.

## How the Template is Used

- Provided as a ready-to-apply manifest by automation or included with test/demo storage assets.
- Users/operators can `kubectl apply -f pvc.yaml` to request shared, persistent storage.
- Integrates with the Rook operator and dynamically creates a volume in the CephFS storage pool.

## Key Variables & Substitutions

- No Jinja2 dynamic substitutions—template is static.

## Rendered Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: cephfs-pvc
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: rook-cephfs
```

## Special Logic

- Assigns storageClassName rook-cephfs for dynamic volume management.
- Enables multiple pods to mount this volume (ReadWriteMany).

## Related Files

- Consumed together with cluster.j2, filesystem.j2, storageclass.j2 (CephFS storage stack).
- A core storage request object for rook/ceph-based clusters.
