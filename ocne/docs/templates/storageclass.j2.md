# storageclass.j2 Template Documentation

## Purpose

Defines a Kubernetes StorageClass (rook-cephfs) for dynamic provisioning of CephFS-backed PersistentVolumes using the Rook CSI driver.

## How the Template is Used

- Applied to clusters (often by automation) so that PersistentVolumeClaim objects referencing storageClassName: rook-cephfs will result in dynamic Ceph volumes.
- Connects the CSI/Rook provisioner to the Ceph cluster, providing a central interface for persistent/shared storage.

## Key Variables & Substitutions

- No Jinja2 dynamic variables; template is static.

## Rendered Example

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: rook-cephfs
provisioner: rook.cephfs.csi.ceph.com
parameters:
  clusterID: rook
  fsName: mycephfs
  pool: mycephfs-replicated
  csi.storage.k8s.io/provisioner-secret-name: rook-csi-cephfs-provisioner
  csi.storage.k8s.io/provisioner-secret-namespace: rook
  csi.storage.k8s.io/controller-expand-secret-name: rook-csi-cephfs-provisioner
  csi.storage.k8s.io/controller-expand-secret-namespace: rook
  csi.storage.k8s.io/node-stage-secret-name: rook-csi-cephfs-node
  csi.storage.k8s.io/node-stage-secret-namespace: rook
reclaimPolicy: Delete
```

## Special Logic

- Assigns all required secret/config keys for CephFS dynamic volume management.
- Sets cluster/pool info consistent with other storage templates.
- `reclaimPolicy: Delete` ensures PVs are removed when claims are deleted.

## Related Files

- Used by: pvc.j2 (for claim/bind).
- Connected with: cluster.j2, filesystem.j2, toolbox.j2 for complete Rook/Ceph FS stack automation.
