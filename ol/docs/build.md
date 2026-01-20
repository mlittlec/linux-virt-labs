# build.yml Documentation

## Purpose & Overview

Automates launching OCI compute instances, with support for both paravirtualized (PV) and iSCSI boot volumes. Configures VNICs, public/private IPs, attaches extra block storage, and handles host inventory.

## When to Use

Use as the main instance provisioning playbook when standing up a set of OL-based VMs in OCI, supporting flexible boot/storage, multiple network types, and readiness for further setup.

## Key Variables & Inputs

- `instance_shape`, `instance_ocpus`, `instance_memory`: Machine specs.
- `my_availability_domain`, `my_compartment_id`, `my_subnet_id`: OCI resource locations.
- `ol_image_id`: Source image for the boot volume.
- `item`: Host definition from inventory.
- `volume_type`: "paravirtualized" or "iscsi".
- `block_devices`, `block_count`, `add_block_storage`, etc.: Block volume controls.

## Task Breakdown

- Conditional logic: Two main flows for instance creation (PV and iSCSI).
- Launches compute instance, sets facts/hostvars, prints results if debug is enabled.
- Fetches and sets public/private IPs for the instance.
- Invokes block.yml (in a loop) to attach and provision block volumes.
- Adds hosts to Ansible in-memory and .ini file inventories to enable post-create configuration.
- Handles host group assignment, SSH configuration, and proxy support.
- Optionally adds a special host-level block volume.
- All retries, error handling, and idempotency included.

## Dependencies & Relationships

- Relies on block.yml for storage attachment logic.
- Requires upstream definition of cloud/network/image parameters.
- Integrated into create_instance.yml (as an included role/task).
- Uses facts, hostvars, and templates (ingress/egress security rules) for dynamic deployment.

## Output/Result

- Full OCI instance deployment, ready for login, automation, and further roles.
