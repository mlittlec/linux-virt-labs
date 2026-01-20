# check_instance_available.yml Documentation

## Purpose & Overview

Checks that newly created lab or cloud instances are accessible via Ansible, performs minimal fact gathering, and prints inventory/state for debugging and workflow readiness.

## When to Use

Run after provisioning or deploying compute/network infrastructure to confirm all hosts are online and accessible to Ansible for further setup or automation.

## Key Variables & Inputs

- Hosts: all:!localhost
- Variables: debug_enabled for optional debugging.
- Includes: default_vars.yml, oci_vars.yml.

## Task Breakdown

- Waits for SSH/Ansible connection readiness for each instance.
- Gathers Ansible facts for each host.
- Prints out in-memory inventory and all facts per host (optional, for debugging/troubleshooting).

## Dependencies & Relationships

- Should be run after playbooks that create or modify host infrastructure (e.g., create_instance.yml).
- Facts are required for downstream configuration tasks (user, SSH, etc).

## Output/Result
