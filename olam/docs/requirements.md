# requirements.yml Documentation

## Purpose & Overview

Specifies all required Ansible Galaxy collections to support the Oracle Linux Automation Manager/ lab automation and infrastructure playbooks. Ensures all playbooks, roles, and modules have compatible and tested dependencies.

## When to Use

Run before executing any playbook by installing all listed collections:

```bash
ansible-galaxy collection install -r requirements.yml
```

This guarantees that your lab automation will not fail due to missing dependencies.

## Collections Listed

- `ansible.posix`
- `community.general`
- `community.postgresql`
- `community.crypto` (locked to ">=2.26.0,<2.26.99")
- `freeipa.ansible_freeipa`
- `community.libvirt`
- `oracle.oci`
- `ovirt.ovirt`
- `containers.podman`

## Dependencies & Relationships

- All playbooks and tasks in Oracle Linux Automation Manager/ depend on these collections for cloud, VM, DB, identity, container, and core automation.

## Output/Result
