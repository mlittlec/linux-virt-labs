# requirements.yml Documentation

## Purpose & Overview

Lists all required Ansible Galaxy collections needed for the OL lab automation suite to function—including OCI, libvirt, community, postgresql, freeipa, and ovirt modules.

## When to Use

Install all required dependencies before running any playbook in the OL project, using

```bash
ansible-galaxy collection install -r requirements.yml
```

to guarantee module compatibility and maximum automation portability.

## Collections Specified

- `ansible.posix`: Provides POSIX utilities for system and SSH config.
- `community.general`: Community-supported utilities for roles, core tasks, and integrations.
- `community.crypto`: Cryptographic and SSH key management (with pinned version constraint).
- `community.postgresql`: For PostgreSQL tasks/roles.
- `freeipa.ansible_freeipa`: FreeIPA support for identity/LAB systems.
- `community.libvirt`: KVM/libvirt/QEMU VM management, local lab modernization.
- `oracle.oci`: Oracle Cloud Infrastructure collection for all cloud resource management.
- `ovirt.ovirt`: For integration with Ovirt-based virtualization.

## Dependencies & Relationships

- Consumed automatically by Ansible and all playbooks in the OL lab.
- Up-to-date dependency management ensures playbooks, roles, and templates are compatible via tested, versioned modules.

## Output/Result
