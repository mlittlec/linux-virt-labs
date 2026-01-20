# fss_pod.j2 — Jinja2 Template for FSS Test Pod (Kubernetes)

This Jinja2 template generates a Kubernetes Pod manifest configured to mount a PersistentVolumeClaim backed by Oracle Cloud File Storage Service (FSS).

## Purpose

- Demonstrates/test access to an NFS-backed PersistentVolumeClaim in a K8s cluster.
- Intended to run a simple log-producing process to verify storage integration.

## Input Variables

- No explicit variables beyond what is expected to be rendered by fss_pvc.j2 and upstream FSS provision logic.

## Main Output

A Kubernetes Pod manifest (`fss-pod.yaml`) with:

- Use of official Oracle Linux 8 container image
- `/data` mount path for persistent storage
- Writes a timestamp to `/data/out.txt` in an infinite loop

## Relationships

- **Rendered by:** `fss_deployments.yml`
- **Depends on:** PVC defined in `fss_pvc.j2` (claimName: fss-pvc)
- **Used with:** OCI FSS CSI driver, example/test deployment after storage provisioning

## Example Output (excerpt)

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

