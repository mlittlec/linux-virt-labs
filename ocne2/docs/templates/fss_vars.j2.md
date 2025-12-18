# fss_vars.j2 — Jinja2 Template for FSS Provisioning Output

This simple Jinja2 template renders a YAML file of variables that define the key facts for an OCI File Storage Service (FSS) deployment.

## Purpose

- Produces a YAML document of FSS resource identifiers and connection information.
- Used to pass outputs from Ansible FSS provisioning to downstream deployments (Kubernetes, Helm, templated manifests).

## Input Variables

| Variable                | Description                           |
|-------------------------|---------------------------------------|
| file_system_id          | OCID of the networked file system     |
| mount_target_ip_address | Private mount target IP address       |
| export_path             | Exported NFS path for mounting        |

## Main Output

Typical YAML output (`fss_vars.yml`) for inclusion/consumption:

```yaml
file_system_id: <file_system_id>
mount_target_ip_address: <mount_target_ip_address>
export_path: <export_path>
```

## Relationships

- **Rendered by:** `create_fss.yml` at the end of FSS provisioning
- **Consumed by:** `fss_deployments.yml` for rendering manifest templates
- **Feeds variables to:** `fss_pv.j2`, `fss_pvc.j2`, `fss_pod.j2`

## Example Usage

After provisioning the FSS stack, this variable file is referenced by
`include_vars` to inject facts throughout the deployment chain.

