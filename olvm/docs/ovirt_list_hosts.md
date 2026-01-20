# ovirt_list_hosts.yml Documentation

## Purpose & Overview

Lists all KVM/oVirt hosts currently registered in the Oracle Linux Virtualization Manager cluster. Used for inventory validation, troubleshooting host state, and reporting available compute resources to operators/admins.

## When to Use

Run after host provisioning, removal, or cluster modifications to audit current lab infrastructure.

## Key Variables & Inputs

- Engine and cluster credentials, host group inventory from upstream playbooks.

## Task Breakdown

- Uses Ansible's oVirt modules to enumerate all active/registered compute/hypervisor hosts.
- Outputs full host details and status for review, audit, or integration with other tools.

## Dependencies & Relationships

- Always up-to-date with the actual Oracle Linux Virtualization Manager cluster state.
- Typically run after ovirt_add_hosts.yml, ovirt_delete_host.yml, or as a health check step.

## Output/Result
