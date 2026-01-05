# provision_olvm_engine_publickey.yml Documentation

## Purpose & Overview

Copies the Oracle Linux Virtualization Manager engine's SSH public key to all KVM nodes in the cluster, facilitating secure agentless setup, host registration, and trust for further automated roles/workflows.

## When to Use

Run after initial engine and host provisioning, and before adding KVM nodes to the cluster or running automation requiring password-less control/registration.

## Key Variables & Inputs

- Public/private key names and locations
- Target KVM hosts in inventory

## Task Breakdown

- Copies or distributes the engine's id_rsa.pub authorized_keys entry to all KVM hosts, updating or configuring as needed for role assignment.
- Optionally validates correct key propagation for security.

## Dependencies & Relationships

- Must run after host/user initialization.
- Often required by ovirt_add_hosts.yml and automated trust establishment steps in Oracle Linux Virtualization Manager/AWX/CI setups.

## Output/Result
