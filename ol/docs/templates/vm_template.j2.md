# vm_template.j2 Documentation

## Purpose & Overview

Defines a parameterized libvirt XML domain for Oracle Linux lab VMs, using Jinja2 variables for name, memory, CPU, disks, networking, and more.

## When to Use

Used by provision_kvm_vm.yml as the basis for creating new KVM VMs in the local lab/infrastructure, ensuring automated and reproducible machine builds.

## Key Variables & Substitutions

- `vm_name`: VM hostname.
- `vm_ram_mb`: Memory size (MB).
- `vm_vcpus`: vCPU count.
- `libvirt_pool_dir`: Directory for qcow2 images and ISO.
- `vm_net`: Name of network for VM NIC.
- Everything else (disk modeling, bus type, ISO filename, etc.) is dynamically generated per-run via upstream Ansible variables/playbooks.

## Structure/Logic

- OS: x86_64, Q35 chipset, hvm type.
- Boot: Hard disk default, CD-ROM (cloud-init ISO) included.
- Devices: Virtio for disk and network, random, QEMU agent, USB, console, controller, and secure defaults.
- Networks: Connects to `vm_net` via <interface>.
- Storage: Attach a qcow2 base disk and cloud-init ISO as CD-ROM for user/SSH setup.
- Fully compatible with KVM/QEMU/libvirt and standardized for Ansible-driven flows.

## Dependencies & Relationships

- Called directly from provision_kvm_vm.yml via `lookup('template', ...)`.
- All runtime variables are injected by parent playbook.
- Must be in sync with the actual disk images, cloud-init renderings, and Ansible inventory being built.

## Output/Result
