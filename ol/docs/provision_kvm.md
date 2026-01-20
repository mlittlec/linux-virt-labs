# provision_kvm.yml Documentation

## Purpose & Overview

Provisions and configures a local KVM hypervisor environment—fully automated install/enable for virtualization packages, cockpit web UI, `firewalld` adjustments, and user access. Handles OL8 and OL9 with best-practice service setup.

## When to Use

Use to convert a fresh Oracle Linux system into a ready-to-use KVM hypervisor for lab/VM automation and local testing, or to reset/reconcile packages and user state on an existing system.

## Key Variables & Inputs

- `username`: User to be given hypervisor and GUI group permissions.
- `create_vm`: If true, child playbooks (provision_kvm_vm.yml) are auto-invoked for OL8/OL9 VMs.
- `default_vars.yml`, image variables: For VM builds.

## Task Breakdown

- Installs required packages for virtualization (differentiated by OL8 vs OL9 stack).
- Enables/starts libvirt, modular service sockets, and cockpit web interface.
- Adjusts `firewalld` to support libvirt control.
- Adds user to correct groups and resets SSH as needed.
- Delegates actual VM imaging and config to provision_kvm_vm.yml ("Deploy VM1"/"Deploy VM2" pattern).
- Focuses on reliability, maintains idempotency, allows repeated safe runs.
- Supports both major OL versions and future OL releases with modular handling.

## Dependencies & Relationships

- Consumed by: create_instance.yml as an imported playbook.
- Calls: provision_kvm_vm.yml for actual VM creation.
- Requires group setup/user creation and at least one base image to copy.

## Output/Result
