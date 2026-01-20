# provision_kvm_vm.yml Documentation

## Purpose & Overview

Automates local virtual machine (VM) creation in a lab environment using community.libvirt and cloud-init, supporting repeatable builds for complex topologies.

## When to Use

Use to create base VMs (OL8, OL9, or other images) on KVM hypervisors in the Oracle Linux lab, ensuring images, meta-data, networking, and user SSH setup are all in place.

## Key Variables & Inputs

- `vm_name`, `base_image_name`, `base_image_url`, `base_image_sha`: Controls image source/naming.
- `libvirt_pool_dir`: Local VM image (qcow2) storage.
- `username`: OS user for SSH and cloud-init setup.
- `groups['all']`: For SSH trust keys.
- `vm_ram_mb`, `vm_vcpus`: Sizing/tuning.
- All variables are typically set upstream (e.g., in provision_kvm.yml playbook).

## Task Breakdown

- Gathers current VMs (prevents duplicate).
- Downloads and verifies the base image.
- Copies the image to a local pool, sets ownership for libvirt usage.
- Creates SSH keypair for cloud-init, user meta-data, and public key.
- Generates cloud-init ISO for nocloud provisioning.
- Defines the VM using a Jinja2-generated libvirt XML template (vm_template.j2).
- Starts the VM and ensures it comes online.
- Optionally deletes temporary file artifacts.

## Dependencies & Relationships

- Called by: provision_kvm.yml (may provision both OL8 and OL9 template-based VMs).
- Uses: vm_template.j2 for XML structure.
- Depends on libvirt, cloud-init, and standard KVM stack.

## Output/Result
