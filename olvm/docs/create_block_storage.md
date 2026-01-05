# create_block_storage.yml Documentation

## Purpose & Overview

Automates creation, configuration, and attachment of block storage domains for lab VMs in Oracle Linux Virtualization Manager, enabling persistent and scalable storage for KVM-based infrastructure.

## When to Use

Execute when creating or extending storage domains for KVM/engine nodes in lab, development, or testing environments where Oracle Linux VMs require extra disks.

## Key Variables & Inputs

- Target storage_name, host, block size, compartment/ad, and instance_id inherited from upstream playbooks.
- Must be part of instance creation or post-provision VM expansion.

## Task Breakdown

- Creates OCI blockstorage volumes named by storage_name.
- Attaches block to target instance with full error/retry handling.
- Records and outputs resulting IDs and resource information as facts for follow-up configuration or mounting.

## Dependencies & Relationships

- Called by build.yml with correct item and host context.
- Requires valid OCI config/profile/variables and instance provisioning prior to execution.

## Output/Result
