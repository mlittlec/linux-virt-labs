# block.yml — Add Block Volumes to OCI Instance

This playbook block is included by other playbooks (commonly `build.yml`) to provision and attach block storage volumes to an Oracle Cloud Infrastructure (OCI) compute instance.

## Purpose

- Automates the creation, registration, and attachment of additional block storage volumes to an OCI instance.
- Used to enable extra data storage or application volumes.

## Requirements

- The Oracle OCI Ansible Collection (`oracle.oci`).
- Variables provided by calling playbook:
  - `my_compartment_id`
  - `my_availability_domain`
  - `instance_id`
  - `block_volume_size_in_gbs`
  - `block_devices`
- The source playbook should set `add_block_storage: true` to enable execution.

## Key Variables

| Variable                   | Description                               |
|----------------------------|-------------------------------------------|
| `my_compartment_id`        | OCID of the compartment                   |
| `my_availability_domain`   | Availability domain for allocation        |
| `instance_id`              | OCID of the target compute instance       |
| `block_volume_size_in_gbs` | Size (GB) of each block volume            |
| `block_devices`            | List of device labels (default: b-f)      |

## Main Tasks

1. **Create block volume:**  
   Provisions a new OCI block volume resource in the specified AD/compartment.

2. **Set volume ID:**  
   Registers the new volume’s OCID for later attachment.

3. **Attach volume:**  
   Attaches the volume to the instance using OCI’s `paravirtualized` type.

- Built-in retry/delay logic for reliable provisioning.

## Relationships

- **Included by:** `build.yml`
- **Needs variables set by:** `create_instance.yml`, `default_vars.yml`
- **Part of main stack:** Used only if `add_block_storage` is set.

## References

- [OCI Block Volume Docs](https://docs.oracle.com/en-us/iaas/Content/Block/Concepts/overview.htm)
- [oracle.oci.oci_blockstorage_volume](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_blockstorage_volume_module.html)
- [oracle.oci.oci_compute_volume_attachment](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_compute_volume_attachment_module.html)

## Example Usage

Typically not called directly. Included by looping in `build.yml` to add `block_count` number of volumes:

```yaml
- name: Add block storage to an instance
  include_tasks: "block.yml"
  loop: "{{ query('sequence', 'start=1 end=' + (block_count) | string) }}"
  vars:
    block_devices:
      - b
      - c
      - d
      - e
      - f
```

