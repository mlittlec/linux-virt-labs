# build.yml Documentation

## Purpose & Overview

Automates provisioning of OCI compute instances for Oracle Linux Virtualization Manager labs, supports custom instance types and sizing, configures public/private/VLAN networking, attaches storage, and prepares all instance facts for downstream automation.

## When to Use

Use as a core step in lab provisioning workflows—run by an orchestrator, e.g., create_instance.yml.

## Key Variables & Inputs

- `my_availability_domain`, `my_compartment_id`, `ol_image_id`, `instance_shape`, `block_devices`
- instance definition via items: type, instance_name, instance_ocpus, instance_memory, etc.
- Networking: my_subnet1_id, my_subnet2_id, my_vlan_id

## Task Breakdown

- Launches a compute instance using Oracle Cloud modules, sets core facts and updates inventory.
- Attaches primary and secondary (private, VLAN) networks as appropriate for each role (engine, kvm).
- Attaches block storage for VM domains via included tasks.
- Handles retries, idempotency, and logging throughout.
- Updates both runtime and file-based inventories for ongoing config.

## Dependencies & Relationships

- Included by higher-level orchestrators (create_instance.yml, etc.).
- Delegates to create_block_storage.yml for storage attachment.
- Synchronizes state via local .ansible-state file.

## Output/Result
