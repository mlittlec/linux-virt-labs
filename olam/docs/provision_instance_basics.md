# provision_instance_basics.yml Documentation

## Purpose & Overview

Prepares newly provisioned instances in the Oracle Linux Automation Manager/ lab for further automation: grows the root filesystem, configures user with password-less sudo and SSH, creates required directories, and ensures proper locale and firewall settings.

## When to Use

Use as one of the first steps after bringing instances online via automation, to guarantee all hosts are ready for subsequent roles/playbooks.

## Key Variables & Inputs

- Hosts: all:!localhost:!remote
- Variables: username, user_default_password, private_key, debug_enabled

## Task Breakdown

- Grows root filesystem using oci-growfs.
- Adds (or confirms) the primary Ansible user, ensures inclusion in sudoers/wheel, and enables password-less sudo.
- Sets up authorized ssh key for secure, agentless automation.
- Creates required directories for Ansible temp data.
- Appends locale settings to .bashrc for shell and downstream process compatibility.
- Optionally logs firewalld rejections to the system journal for diagnostic or troubleshooting needs.

## Dependencies & Relationships

- Consumed by create_instance.yml and invoked for all provisioned lab hosts.
- Reads default_vars.yml and oci_vars.yml for configuration.

## Output/Result
