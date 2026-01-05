# build.yml Documentation

## Purpose & Overview

Automates the creation of an OCI compute instance with robust network, storage, and VNIC configuration for Oracle Linux Automation Manager/Oracle Linux Automation Manager labs.

## When to Use

Run during lab provisioning to instantiate VMs with cloud-agnostic bootstrapping, public/private IP allocation, and storage attachment.

## Key Variables & Inputs

- `my_availability_domain`, `my_compartment_id`, `ol_image_id`, `instance_shape`, `instance_ocpus`, `instance_memory`, `my_subnet_id`, `private_key`.
- References from default_vars.yml and cumulative variables from parent tasks.
- `block_count`/`block_devices` for volume attachment.

## Task Breakdown

- Provisions an OCI compute instance, specifying source image and boot volume size.
- Sets authorized SSH key, agent, and monitoring options.
- Fetches and sets up VNICs for public/private IP mapping.
- Registers and maintains instance display_name and instance_id.
- Includes tasks from block.yml to dynamically attach storage.
- Adds hosts to both Ansible's dynamic inventory and hosts file for integration with follow-up playbooks.
- Optionally prints instance connection info for debugging or tracking.

## Dependencies & Relationships

- Must be called with valid OCI and SSH configuration in-place.
- Calls block.yml for storage extension.
- Linked via parent orchestrator to overall lab creation/teardown logic.

## Output/Result
