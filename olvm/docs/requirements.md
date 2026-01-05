# requirements.yml Documentation

## Purpose & Overview

Defines the Ansible Galaxy collections needed to run all automation in the Oracle Linux Virtualization Manager lab directory—ensures all network, VM, storage, and platform tasks have tested, compatible dependencies.

## When to Use

Install these collections before any major playbook or role run:

```bash
ansible-galaxy collection install -r requirements.yml
```

For reproducibility and to avoid module-not-found errors.

## Collections Listed

- ansible.posix, community.general, freeipa.ansible_freeipa, community.crypto, oracle.oci, ovirt.ovirt, containers.podman, and others depending on feature set.

## Dependencies & Relationships

- Every playbook and role in the cluster runs against these tested module versions.
- YAML role/collection versions can be updated for new features or environments as needed.

## Output/Result
