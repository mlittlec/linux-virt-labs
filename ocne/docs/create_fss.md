# create_fss.yml Documentation

## Purpose & Overview

Automates the end-to-end setup of Oracle Cloud Infrastructure (OCI) File Storage Service (FSS) for cluster or NFS use. This playbook provisions mount targets, discovers required IDs and IPs, creates a backing file system, sets up an export path, and documents discovered connection parameters for subsequent workloads.

## When to Use

Use this playbook when:

- A shared network file system is required for OCI-based Kubernetes, Oracle Linux, or other distributed workloads.
- You want to ensure all FSS resources (mount target, file system, and export) are provisioned and recorded in one step.

## Key Variables & Inputs

- `my_availability_domain`: OCI AD for FSS resources.
- `my_compartment_id`: OCI compartment OCID.
- `my_subnet_id`: Subnet OCID for the mount target.
- `debug_enabled`: Enables step-by-step output for monitoring or troubleshooting.

## Task Breakdown

- **Provision FSS Mount Target:** Creates a mount target using `oci_file_storage_mount_target`.
- **Debug Output:** Optionally prints results.
- **Set Facts:** Extracts mount target, export set, and private IP IDs.
- **Private IP Lookup:** Retrieves mount target primary IP address.
- **Provision FSS File System:** Creates a file system for storage.
- **Export Creation:** Sets up a file system export making `/fss-ocne` accessible to cluster nodes.
- **Set/Write FSS Vars:** Records all generated/assigned values in a vars file via templating (`fss_vars.j2`).

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: `oci_file_storage_mount_target`, `oci_file_storage_file_system`, `oci_file_storage_export`, and network/private IP modules.
- Relies on variables produced here for matching mount operations in subsequent playbooks.
- Uses the templated file `fss_vars.j2` for writing generated connection values.

## Output/Result

- A mounted, exported, addressable OCI File Storage resource.
- `fss_vars.yml` containing all IDs, IPs, and paths required by following steps.

## Example Run

```bash
ansible-playbook create_fss.yml \
  -e "my_compartment_id=..." \
  -e "my_availability_domain=..." \
  -e "my_subnet_id=..."
