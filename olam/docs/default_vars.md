# default_vars.yml Documentation

## Purpose & Overview

Central variable definition file for Oracle Linux Automation Manager (OLAM) labs: describes hosts, images, instance types/volume sizes, credentials, toggles for all feature components (VNC, builder, PAH, FreeIPA, Git, etc.), and configuration for advanced cases (KVM, VNC, root, etc.).

## When to Use

Always included as a vars_files entry in parent playbooks; edit or override for lab customization, QA, demo, or training scenarios.

## Key Variables & Inputs

- `compute_instances`: Host inventory and configs
- `os`, `os_version`: Kernel image selection
- `instance_shape`, `instance_ocpus`, `instance_memory`, `boot_volume_size_in_gbs`
- All toggles: `use_*` for PAH, builder, FreeIPA, Git, KVM, VNC, password-less_ssh, and more
- Cloud and KVM image URLs and hashes
- Credentials for Oracle Linux Automation Manager/Ansible, AWX admin, database, etc.
- Network/domain/proxy variables and sample proxy_env and pip_proxy_env

## Task Breakdown

- Exported as a variable file to any play needing resource or deployment context.
- Models all required settings for building, configuring, and tearing down complete Oracle Linux Automation Manager, builder, and DevOps environments.
- All secrets/example passwords can be overridden at runtime.

## Dependencies & Relationships

- Read by every major playbook in olam/.
- Directly referenced by most templates in the lab for runtime dictionary lookups.

## Output/Result
