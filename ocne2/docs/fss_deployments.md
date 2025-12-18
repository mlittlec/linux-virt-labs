# fss_deployments.yml — Create FSS-backed Kubernetes Resources

This playbook generates Kubernetes deployment manifests (PersistentVolume, PersistentVolumeClaim, Pod) for use with Oracle Cloud File Storage Service (FSS) as persistent backend storage.

## Purpose

- Dynamically generates the Kubernetes YAML resources needed to mount and consume FSS storage in a K8s cluster.
- Provides bridge logic between OCI-provisioned FSS and applications running in Kubernetes.

## Requirements

- Variable file (`fss_vars.yml`) rendered by previous FSS provisioning.
- Jinja2 templates: `fss_pv.j2`, `fss_pvc.j2`, `fss_pod.j2`.
- Typically run within the context of a larger Oracle CNE or Kubernetes deployment scenario.

## Main Tasks

1. **Include FSS variables:**  
   Loads the YAML with vital identifiers rendered during FSS provisioning.

2. **Render Kubernetes resources:**  
   Uses Jinja2 templates to create files:
   - `fss-pv.yaml`: PersistentVolume bound to FSS
   - `fss-pvc.yaml`: PersistentVolumeClaim for Pod usage
   - `fss-pod.yaml`: Demonstration/test pod configured to mount the PVC

3. **Permissions:**  
   All files are owned by the relevant user and given ready-to-consume permissions for deployment (`kubectl apply -f ...`).

## Relationships

- **Produces:** Artifacts for K8s cluster deployment of FSS-backed storage.
- **Used by:** `deploy_ocne_oci.yml` and other playbooks when storage integration is required.
- **Consumes:** Output of `create_fss.yml` and Jinja2 templates.

## References

- [Kubernetes Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- [OCI FSS CSI Driver](https://github.com/oracle/oci-cloud-controller-manager/blob/master/docs/flexvolume/flexvolume.md)

## Example Output

After execution, the following files will exist for deployment:

- `~/fss-pv.yaml`
- `~/fss-pvc.yaml`
- `~/fss-pod.yaml`

