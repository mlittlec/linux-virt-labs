# build.yml Documentation

## Purpose & Overview

Launches a new Oracle Cloud Infrastructure (OCI) compute instance, configures its network interfaces, optionally attaches storage, and registers the instance for subsequent Ansible automation workflows. Includes support for both direct subnet and VLAN setups, and can provision additional block volume storage for worker nodes.

## When to Use

Use this playbook when:

- You need to automate the provisioning of compute instances on OCI as part of a Kubernetes or Oracle Linux infrastructure deployment.
- You want to ensure every new VM is created, networked, optionally VLAN-attached, and storage-augmented in a repeatable, debug-friendly way.

## Key Variables & Inputs

- `my_availability_domain`: OCI AD for the instance.
- `my_compartment_id`: OCI compartment OCID.
- `my_subnet_id`: Target subnet OCID for primary VNIC.
- `ol_image_id`: Image OCID for the new instance.
- `instance_shape`: OCI shape (resource allocation profile).
- `instance_ocpus`, `instance_memory`: vCPU and RAM config.
- `private_key`: SSH private key name (for generating authorized_keys).
- `item.value.instance_name`: Optional, controls hostname.
- `use_vlan`, `my_vlan_id`: If attaching VLAN VNICs.
- `add_ceph_block_storage`, `ceph_volume_size_in_gbs`: If adding Ceph block storage to worker nodes.
- `debug_enabled`: Toggle debug/info output.
- Additional vars: see playbook for all details; many are templated per host/item.

## Task Breakdown

- **Launch Instance:** Uses `oracle.oci.oci_compute_instance` to provision a new compute VM with defined resources, SSH keys, monitoring/management config.
- **Show Details:** Prints result if `debug_enabled`.
- **Set Facts:** Saves instance OCID, display name for downstream tasks.
- **Get VNIC Info:** Fetches and assigns both private and public IPs, introspects OCI VNIC attachments.
- **VLAN Support:** Optional creation/attachment of VLAN VNICs if `use_vlan`.
- **Ceph Block Volumes:** Optionally attaches OCI block storage for Ceph to worker instances if enabled.
- **Output:** Prints important IPs and identifiers for workflow use, if `debug_enabled`.
- **Register Host:** Adds VM details to Ansible's runtime host inventory for further playbook tasks.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection, incl. `oci_compute_instance`, `oci_blockstorage_volume`, `oci_compute_volume_attachment`.
- Consumes variables provided at group/host level or passed via extra-vars.
- Optionally invokes Jinja2 templates as referenced by related post-creation provisioning playbooks (not directly in this file).

## Output/Result

- A new compute instance, optionally VLAN-attached and/or equipped with Ceph storage.
- Adds instance to Ansible host file and sets necessary variables for future steps.

## Example Run

```bash
ansible-playbook build.yml \
  -e "my_availability_domain=..." \
  -e "my_compartment_id=..." \
  -e "my_subnet_id=..." \
  -e "ol_image_id=..." \
  -e "instance_shape=VM.Standard.E4.Flex" \
  -e "instance_ocpus=2" \
  -e "instance_memory=16"
