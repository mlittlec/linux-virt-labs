# fss_pod.j2 Template Documentation

## Purpose

Provides a Kubernetes Pod manifest that tests persistent storage via a mounted OCI File Storage Service (FSS) PersistentVolumeClaim.

## How the Template is Used

- Rendered and distributed to control plane nodes by fss_deployments.yml.
- Demonstrates correct pod-to-PVC-to-PV-to-FSS integration in the Kubernetes cluster.
- Used by operators/testers to verify persistent storage setup for the lab is functional—writes timestamps to /data/out.txt on the NFS mount.

## Key Variables & Substitutions

- No Jinja2 variables or substitutions; this template is fully static.

## Rendered Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  containers:
  - name: app
    image: ghcr.io/oracle/oraclelinux:8
    command: ["/bin/bash"]
    args: ["-c", "while true; do echo $(date -u) >> /data/out.txt; sleep 5; done"]
    volumeMounts:
    - name: persistent-storage
      mountPath: /data
  volumes:
  - name: persistent-storage
    persistentVolumeClaim:
      claimName: fss-pvc
```

## Special Logic

- Mounts /data from a PVC named fss-pvc.
- Infinite loop writes timestamps to persist data; pod can be deleted/recreated and data will persist.

## Related Files

- Rendered by: fss_deployments.yml.
- Consumes: fss-pvc.yaml, which defines the PVC.
- Used for: Storage integration validation and demonstration.
