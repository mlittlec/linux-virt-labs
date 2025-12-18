# provision_podman.yml — Install Podman and Container Tools

This playbook installs the required container tooling on OCI compute instances, supporting modern OCI-based development and Kubernetes workflows using Podman.

## Purpose

- Installs Podman and related tools on Oracle Linux 8 or 9.
- Ensures containerization support for running application containers or deploying with Kubernetes on provisioned hosts.

## Requirements

- Target hosts: Oracle Linux 8 or 9 in the `server` group.
- Variables set in `default_vars.yml`.
- `ansible.posix` and relevant collections in `requirements.yml`.

## Main Tasks

1. **Check OS version:**  
   Selects the correct package group or list for Oracle Linux 8 vs 9.

2. **Install packages:**  
   - Oracle Linux 8: Installs the `@container-tools:ol8` group, conntrack, curl.
   - Oracle Linux 9: Installs podman, podman-docker, conntrack, curl.

## Relationships

- **Feature flag:** Controlled by `use_podman` in `default_vars.yml`.
- **Run from:** The main orchestration playbook (`create_instance.yml`) if enabled.

## References

- [Podman Documentation](https://podman.io/)
- [Containers on Oracle Linux](https://docs.oracle.com/en/operating-systems/oracle-linux/podman/index.html)
- [Ansible dnf module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/dnf_module.html)

## Example Usage

```yaml
- name: Provision Podman and Container Tools
  import_playbook: provision_podman.yml
  when: use_podman

