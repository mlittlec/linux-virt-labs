# provision_kvm.yml Documentation

## Purpose & Overview

Automates installation/configuration of a KVM (libvirt/QEMU) hypervisor on a dedicated `kvm-server` laboratory node in Oracle Linux Automation Manager labs, handling service, group, and firewall settings.

## When to Use

Run to bootstrap a testbed hypervisor for VM lab/dev clusters when Oracle Linux 8/9 is used as the OS platform.

## Key Variables & Inputs

- Hosts: kvm-server
- All package names are conditional on OS major version.
- `username` (for group membership, authority)

## Task Breakdown

- Installs `@virt` group plus tooling for virtualization, containerization, and cockpit management on OL8; modular packages for OL9.
- Enables and starts required libvirt and cockpit services/sockets.
- Opens firewall ports for libvirt access.
- Adds user to libvirt/qemu groups for non-root virtualization and resets SSH.

## Dependencies & Relationships

- May be called by create_instance.yml (as an import) or on-demand for base hypervisor role install.
- VM creation and testing must follow, using a compatible KVM playbook or tool.

## Output/Result
