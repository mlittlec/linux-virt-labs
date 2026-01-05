# block.yml Documentation

## Purpose & Overview

Provisions and attaches block storage volumes to Oracle Cloud Infrastructure (OCI) compute instances. Handles multiple attachment scenarios: batch or per-item, using both global and item-level toggles.

## When to Use

Use within other automation flows (e.g., build.yml, create_instance.yml) to automate, control, and trace all block volume provisioning and instance attachment in OCI.

## Key Variables & Inputs

- `block_devices`: List of device letters (e.g., ['b','c','d'])—controls device naming.
- `my_availability_domain`, `my_compartment_id`: OCI context values to locate all resources.
- `block_volume_size_in_gbs`: Size of each volume.
- `instance_id`, `volume_type`: Target compute instance and attachment type.
- `add_block_storage`, `item.value.add_bv`: Toggles: global and per-item.
- `block_count`: Number of block devices to attach.

## Task Breakdown

- Creates a new OCI blockstorage volume per device, naming each uniquely (blockvolume-vdX).
- Records the volume OCID for downstream usage.
- Attaches each volume to the running instance with a uniquely named device path, ensuring ordering/safety.
- Supports both all-host (add_block_storage) and itemized (item.value.add_bv) modes.
- Robust retries and idempotency baked in.

## Dependencies & Relationships

- Used by: build.yml (via include_tasks), create_instance.yml (via imported roles/tasks).
- Requires Oracle OCI Ansible collection.
- Must run after the target compute instance is available in OCI.

## Output/Result

- One or more OCI block volumes per instance, named/attached for full OS/automation visibility.
