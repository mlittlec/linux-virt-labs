# fss_pvc.j2 — Jinja2 Template for FSS PersistentVolumeClaim (Kubernetes)

This Jinja2 template generates a Kubernetes PersistentVolumeClaim (PVC) manifest, which binds to an FSS-backed PersistentVolume for use by cluster workloads.

## Purpose

- Allows application pods to request and consume persistent storage provisioned and managed using Oracle Cloud FSS.
- Ensures decoupling of application and storage layers with flexible volume configuration.

## Input Variables

- References the PV created by `fss_pv.j2` (volumeName: fss-pv)

## Main Output

- Kubernetes PersistentVolumeClaim YAML (`fss-pvc.yaml`)
  - Standard `ReadWriteMany` access
  - Size and class match the FSS backend
  - Binds explicitly to the `fss-pv` volume

## Relationships

- **Rendered by:** `fss_deployments.yml` alongside PV and pod resources
- **Claimed by:** Pods such as those generated from `fss_pod.j2`
- **Depends on:** FSS PV manifest defined by `fss_pv.j2`

## Example Output

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

