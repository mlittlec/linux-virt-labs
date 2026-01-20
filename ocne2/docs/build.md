# build.yml — Launch and Configure OCI Compute Instance

This playbook provisions an Oracle Cloud Infrastructure (OCI) compute instance, configures its networking, attaches block storage volumes (by including `block.yml`), and sets up in-memory inventory for subsequent playbooks.

## Purpose

- Launches a compute instance on OCI using provided parameters and variables
- Attaches one or more block storage volumes for additional data/application storage
- Collects and stores instance metadata (OCIDs, IP addresses) for orchestration
- Prepares new host for further configuration and Ansible-driven automation

## Requirements

- Oracle OCI Ansible Collection (`oracle.oci`)
- Variables provided as part of Ansible inventory or the parent playbook:
  - `my_compartment_id`
  - `my_availability_domain`
  - `ol_image_id`
  - `instance_shape`
  - `my_subnet_id`
  - `block_count` and `block_volume_size_in_gbs`
  - SSH public key in `<home>/.ssh/<private_key>.pub`
- Optional:
  - `debug_enabled` toggles extra log/debug output
  - Additional host and instance group variables

## Key Variables

| Variable                   | Description                                           |
|----------------------------|-------------------------------------------------------|
| `my_compartment_id`        | OCID of target compartment                            |
| `my_availability_domain`   | Availability domain                                   |
| `ol_image_id`              | OCID of Oracle Linux image for boot volume            |
| `instance_shape`           | OCI compute shape                                     |
| `block_count`              | Number of block volumes to attach                     |
| `block_volume_size_in_gbs` | Size per block storage volume                         |
| `private_key`              | SSH private key name for host access                  |
| `item.value.*`             | Per-instance override variables (see parent playbook) |

## Main Tasks

1. **Launch instance:**  
   Provisions a new VM on OCI per specified variables (network, OS, resources).

2. **Set and print facts:**  
   Registers OCIDs, private/public IPs, and details for in-memory and downstream use.

3. **Attach block storage:**  
   Includes `block.yml` per `block_count` for extra volume provisioning.

4. **Configure Ansible inventory:**  
   Dynamically adds launched instance to in-memory `add_host` inventory group for later playbook/role execution.

## Relationships

- **Invokes:** `block.yml` to add block storage
- **Registered variables used by:** `create_instance.yml` and sub-sequent configuration
- **Dependent on:** Variables from `default_vars.yml`, parent calls in `create_instance.yml`

## References

- [oracle.oci.oci_compute_instance](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/collections/oracle/oci/oci_compute_instance_module.html)
- [add_host module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/add_host_module.html)

## Example Usage

Called/looped within `create_instance.yml` for each defined instance:

```yaml
- name: Build an instance
  include_tasks: "build.yml"
  loop: "{{ lookup('dict', compute_instances, wantlist=True) }}"

