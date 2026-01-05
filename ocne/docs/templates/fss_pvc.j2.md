# fss_pvc.j2 Template Documentation

## Purpose

Defines a Kubernetes PersistentVolumeClaim (PVC) resource that binds to the statically-created FSS PersistentVolume, allowing pods to request and consume persistent NFS storage in Oracle Cloud.

## How the Template is Used

- Rendered and distributed by fss_deployments.yml.
- Used by demo/test pods (see fss_pod.j2) and any other K8s workload requiring shared, persistent storage.
- Matches to a single provisioned PV (`fss-pv`) for static NFS mounting.

## Key Variables & Substitutions

- No Jinja2 variables are present; purely static manifest.

## Rendered Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: fss-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 50Gi
  volumeName: fss-pv
```

## Special Logic

- `storageClassName: ""` ensures binding to a statically-created PV (not dynamic provisioning).
- Binds exactly to the previously defined PV (`volumeName: fss-pv`).
- Allows multiple pods to mount (ReadWriteMany).

## Related Files

- Rendered by: fss_deployments.yml.
- Consumed by: pods such as fss_pod.j2.
- Binds to: fss_pv.j2-generated PV resource.
