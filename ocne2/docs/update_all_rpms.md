# requirements.yml — Ansible Collection Dependencies

This file specifies all the required Ansible Galaxy collections needed to support the automation, OCI, OS, cryptography, and infrastructure features used by the playbooks in this project.

## Purpose

- Ensures that all playbooks have access to the correct modules, plugins, and roles available from Ansible Galaxy.
- Provides a version-locking and reproducible installation method for third-party code dependencies.

## Major Collections

| Collection                        | Modules Used/Features                   |
|-----------------------------------|-----------------------------------------|
| ansible.posix                     | Filesystem, authorized_key, command     |
| community.general                 | Utility plugins, ini lookup, random_str |
| community.crypto                  | OpenSSH keypair generation, cryptotasks |
| freeipa.ansible_freeipa           | Optional, for FreeIPA integration       |
| community.libvirt                 | Libvirt/KVM resource management         |
| oracle.oci                        | Core OCI resource automation modules    |
| ovirt.ovirt                       | Ovirt virtualization management         |
| community.postgresql              | Postgres support (optional)             |

## Example Usage

Install all required dependencies:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Relationships

- **Required by:** All Ansible playbooks in this OCNE/OCI automation stack.
- **Referenced in:** Setup/onboarding guides, `README.md`.

## Additional Information

For specific collection docs and supported modules, see the [Ansible Galaxy documentation](https://docs.ansible.com/ansible/latest/collections/index.html).

