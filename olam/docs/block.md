# block.yml Documentation

## Purpose & Overview

Provisions and attaches one or more OCI block storage volumes to a target compute instance, using Ansible's cloud modules for robust, repeatable storage automation.

## When to Use

Use as part of an automation workflow where instances require custom or expanded storage, for shared data, persistent application state, or advanced IO benchmarking.

## Key Variables & Inputs

- `add_block_storage`: Boolean, drives whether to provision blocks.
- `my_compartment_id`, `my_availability_domain`: OCI references.
- `item.value.instance_name`, `timestamp`: Used to generate unique volume names.
- `block_volume_size_in_gbs`: Size of each block.
- `my_instance_id`: Target for attachment.
- `block_devices`: List of disk suffixes (e.g., [b, c, d]) for device names.
- `ansible_loop.index0`: Index for naming/mounting.

## Task Breakdown

- Creates a uniquely named OCI block storage volume for each required device.
- Sets a fact for volume_id for downstream reference.
- Attaches each block to the compute instance (as paravirtualized).
- All steps are robustly guarded with retries and fail handling.

## Dependencies & Relationships

- Typically included by main provisioning playbooks (build.yml, create_instance.yml).
- Uses OCI config/profile variables for secure connection.
- Requires a running compute instance and OCI Ansible collection.
- Variable block_devices tied to the number and order of disks per host.

## Output/Result
