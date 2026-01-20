# provision_instance_basics.yml Documentation

## Purpose & Overview

Handles foundational configuration for a newly provisioned VM or host in Oracle Linux Virtualization Manager labs: expands root FS, sets up user accounts, ensures password-less sudo and SSH, and configures locale and temporary directories.

## When to Use

Run as one of the first steps after bringing KVM/engine nodes online, ensuring their environments are lab-ready for automation.

## Key Variables & Inputs

- username, user_default_password, private_key, debug_enabled, etc.
- Inherits host groups and variables from higher-level provisioning plays.

## Task Breakdown

- Expands root partition (oci-growfs).
- Creates/configures user with sudo and authorized key.
- Sets password-less sudo access.
- Creates Ansible temp directory as user.
- Adds locale settings and enables denied packet logging in firewall if needed.

## Dependencies & Relationships

- Consumed by create_instance.yml and checked by subsequent automation.
- Reads from default_vars.yml and oci_vars.yml.

## Output/Result
