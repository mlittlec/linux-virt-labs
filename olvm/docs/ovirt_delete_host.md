# ovirt_delete_host.yml Documentation

## Purpose & Overview

Automates removal of a KVM/oVirt/Oracle Linux Virtualization Manager host from the cluster. Ensures accurate, safe clean-up for decommissioning, migration, or failure scenarios.

## When to Use

Run to remove stale, failed, or decommissioned hosts from an Oracle Linux Virtualization Manager cluster, ensuring environment health and management confidence.

## Key Variables & Inputs

- Host credentials and inventory (target host must be identified)
- Any cluster-specific locks or HA states must be handled

## Task Breakdown

- Uses Ansible oVirt modules to delete a host from the engine.
- Optionally waits or runs post-removal checks to confirm success.

## Dependencies & Relationships

- Relies on previous host and cluster setup (host must exist in engine before deletion).
- May be triggered by teardown routines or as a manual admin action.

## Output/Result
