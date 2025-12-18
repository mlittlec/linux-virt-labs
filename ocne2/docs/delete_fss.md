# delete_fss.yml — Teardown Oracle Cloud File Storage Service (FSS)

This playbook performs safe teardown of any previously-provisioned OCI File Storage Service (FSS) resources, including the export, mount target, and file system objects.

## Purpose

- Ensures all FSS-related resources are fully removed (state=absent).
- Prevents cloud resource leaks and potential unexpected charges.
- Typically included as a teardown or cleanup step from a parent playbook (e.g., `create_instance.yml`).

## Requirements

- Oracle OCI Ansible Collection (`oracle.oci`).
- Variables set from the provisioning stage:
  - `export_id`, `mount_target_id`, `file_system_id`.

## Main Tasks

1. **Delete Export:**  
   Removes the NFS export from the given FSS.
2. **Delete Mount Target:**  
   Cleans up the mount target, breaking NFS client connectivity.
3. **Delete File System:**  
   Removes the primary network file system container.

## Relationships

- **Teardown phase:** Called by `create_instance.yml` during environment teardown, if `use_fss` is enabled.
- **Depends on:** Variable facts produced by `create_fss.yml`.

## Reference

- [oracle.oci.oci_file_storage_export](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_file_storage_export_module.html)
- [oracle.oci.oci_file_storage_mount_target](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_file_storage_mount_target_module.html)
- [oracle.oci.oci_file_storage_file_system](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_file_storage_file_system_module.html)

## Example Usage

```yaml
- name: Delete file system storage
  include_tasks: "delete_fss.yml"
  when: use_fss

