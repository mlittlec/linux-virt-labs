# create_instance.yml Documentation

## Purpose & Overview

Top-level playbook for orchestrating all stages of deployment in the Oracle Linux automation lab: networking, VM provisioning (OCI/cloud and local), configuration, package update, host setup, destruction, and more.

## When to Use

Use this playbook to provision full test/deployment environments in OCI or on local hypervisors, handle all configuration, SSH, and variable orchestration, or to destroy/deprovision when teardown is needed.

## Key Variables & Inputs

- Extensive: see default_vars.yml for base variables including compute_instances, OS/image specs, resource group toggles, storage, VM parameters, toggles for VNC/KVM/Podman/VirtualBox, and more.
- Uses inventory groups (server/vbox/etc) to control scope.

## Task Breakdown

- **Facts & Cloud State Discovery:** Reads OCI config and facts, compartment, region, AD, builds network, determines images, prepares variables.
- **Network & Security Provisioning:** VCN/subnet setup, route tables, security lists, ingress/egress via Jinja2 templates.
- **Provisioning Steps:**
  - Launches VMs (via build.yml), sets up hosts, SSH, proxy, group assignment.
  - Password-less SSH and key setup (password-less_setup.yml, host_setup.yml).
  - KVM/PV/Local hypervisor deployment (imports/executes child playbooks for KVM, VirtualBox etc. as needed).
  - Handles VNC/GNOME provisioning, ISO download, etc, by importing associated playbooks.
  - Outputs result details and SSH info for further automation.
  - Optional controlled pause step.
- **Teardown/Destruction:**
  - Designed for safe teardown of all cloud and local resources (with explicit fail checks, resource removal, and state cleanup).

## Dependencies & Relationships

- Imports and delegates to: build.yml, host_setup.yml, passwordless_setup.yml, download_ol_iso.yml, provision_kvm.yml, provision_podman.yml, provision_vnc.yml, provision_vbox.yml, update_all_rpms.yml.
- Uses egress_security_rules.j2, ingress_security_rules.j2, oci_vars.j2, block.yml.
- Requires all dependencies present in requirements.yml.

## Output/Result

- Fully provisioned, multi-VM, networked lab environment, ready for application workloads, admin, or teardown.
- Inventory, SSH, and config primed for extension or further automation.
