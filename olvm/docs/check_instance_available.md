# check_instance_available.yml Documentation

## Purpose & Overview

Ensures newly created engine and KVM instances are accessible via SSH, gathers minimal facts, and optionally prints inventory and variable info for debugging.

## When to Use

Run after provisioning to validate that all virtualized infrastructure is online, networked, and ready for downstream automated configuration.

## Key Variables & Inputs

- Hosts: engine, kvm (excluding localhost)
- Variables from default_vars.yml and oci_vars.yml
- debug_enabled (verbosity toggle)

## Task Breakdown

- Waits for SSH/Ansible connection for each host via port 22.
- Gathers all available facts for each instance for inventory and audit.
- Optionally prints group and hostvars for troubleshooting/config validation.

## Dependencies & Relationships

- Should be triggered after build.yml or instance bring-up workflow.
- Prerequisite for further task orchestration requiring fact collection.

## Output/Result
