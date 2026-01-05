# ovirt_check_hosts.yml Documentation

## Purpose & Overview

Verifies KVM/oVirt hosts are reachable, healthy, and capable of participating in Oracle Linux Virtualization Manager clusters. Checks connectivity, authentication, status, and configuration readiness.

## When to Use

Run after provisioning/adding new hosts to validate cluster integrity before VM deployment, storage attach, or role expansion.

## Key Variables & Inputs

- Host credentials, cluster details, inventory provided by orchestrator playbook/group.

## Task Breakdown

- Uses Ansible's oVirt modules to check host reachability and configuration.
- Produces debug output for immediate operational troubleshooting.

## Dependencies & Relationships

- Recommended to run after ovirt_add_hosts.yml and before VM creation.
- Integrated with larger Oracle Linux Virtualization Manager install/test flows.

## Output/Result
