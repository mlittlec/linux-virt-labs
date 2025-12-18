# create_fss.yml — Provision Oracle Cloud File Storage Service (FSS)

This playbook provisions OCI File Storage (FSS) for use as network-attached storage for compute instances and Kubernetes clusters.

## Purpose

- Automates creation and configuration of an FSS file system, mount target, and export.
- Gathers all identifiers (OCIDs, private IPs) for use in subsequent infrastructure and application stacks.
- Renders variables file (`fss_vars.yml`) for downstream consumption (e.g., Kubernetes volumes).

## Requirements

- Oracle OCI Ansible Collection (`oracle.oci`).
- Variables from calling playbook:
  - `my_availability_domain`
  - `my_compartment_id`
  - `my_subnet_id`
- Optional: `debug_enabled` for additional output.

## Key Variables

| Variable                  | Description                                          |
|---------------------------|------------------------------------------------------|
| `my_availability_domain`  | Target AD for storage                                |
| `my_compartment_id`       | Compartment for all objects                          |
| `my_subnet_id`            | Subnet for mounting FSS                              |
| `mount_target_id`         | ID of the mount target (auto-set)                    |
| `mount_target_ip_address` | Private IP for mount operations (auto-set)           |
| `file_system_id`          | OCID for the file system (auto-set)                  |
| `export_id`               | OCID for the FSS export (auto-set)                   |
| `export_path`             | Path for the NFS export (auto-set)                   |

## Main Tasks

1. **Provision mount target:**  
   Creates an OCI mount target in a subnet for access.

2. **Provision file system:**  
   Allocates the FSS file system in the same AD/compartment.

3. **Export configuration:**  
   Sets export rules for global read-write NFS access.

4. **Outputs variables:**  
   Generates `fss_vars.yml` for use in Kubernetes or downstream Ansible tasks.

## Relationships

- **Referenced by:** `create_instance.yml` (teardown in `delete_fss.yml`)
- **Produces variables for:** Jinja2 templates (`fss_pv.j2`, `fss_pvc.j2`, `fss_pod.j2`)
- **Main consumer:** Kubernetes storage provisioning via `fss_deployments.yml`

## References

- [OCI File Storage Service](https://docs.oracle.com/en-us/iaas/Content/File/Concepts/filestorageoverview.htm)
- [oracle.oci.oci_file_storage_mount_target](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_file_storage_mount_target_module.html)
- [oracle.oci.oci_file_storage_file_system](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_file_storage_file_system_module.html)
- [oracle.oci.oci_file_storage_export](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_file_storage_export_module.html)

## Example Usage

```yaml
- name: Set file system storage
  include_tasks: "create_fss.yml"
  when: use_fss

