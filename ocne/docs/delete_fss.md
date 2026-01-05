# delete_fss.yml Documentation

## Purpose & Overview

Destroys File Storage Service (FSS) resources in Oracle Cloud Infrastructure (OCI) that were previously provisioned for the cluster or NFS workloads. The playbook ensures that all components—export path, mount target, and file system—are deleted safely and in correct dependency order.

## When to Use

Run this playbook when:

- Tearing down or resetting the OCI lab/test/development environment.
- Decommissioning infrastructure after automation testing to prevent resource leakage and avoid charges.

## Key Variables & Inputs

- `export_id`: The ID of the FSS export path to delete.
- `mount_target_id`: The ID of the mount target to remove.
- `file_system_id`: The ID of the backing file system to delete.

## Task Breakdown

- **Export Deletion:** Deletes the FSS export—removes the exported file system access path.
- **Mount Target Deletion:** Deletes the FSS mount target object from the given subnet.
- **File System Deletion:** Destroys the actual persistent file storage volume.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: file storage modules.
- The identified IDs must be present (generally gathered from create_fss.yml/fss_vars.yml).
- Called as part of broad teardown (e.g., from create_instance.yml's cleanup/teardown phase).

## Output/Result

- All referenced FSS resources deleted and cleaned from OCI compartment.
- No file system artifacts, orphaned mounts, or cloud storage bills.

## Example Run

```bash
ansible-playbook delete_fss.yml \
  -e "export_id=ocid1.export.oc1..." \
  -e "mount_target_id=ocid1.mounttarget.oc1..." \
  -e "file_system_id=ocid1.filesystem.oc1..."
