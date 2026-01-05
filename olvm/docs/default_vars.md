# default_vars.yml Documentation

## Purpose & Overview

Holds all global variables and toggles for Oracle Linux Virtualization Manager lab deployments, covering instance configuration, network setup, storage, KVM/engine role toggles, cloud resource values, and provisioning flags.

## When to Use

Always include as a vars_files in orchestrator and provisioning playbooks for consistent, reproducible deployments.

## Key Variables & Inputs

- Core instance variables (compute_instances, shape, ocpus, memory, images)
- Storage, SSH, user, and password fields
- Networking (subnets, VCN, VLAN)
- Tuning for HA, DevOps, clustering, firewalls, proxy, and feature toggles

## Task Breakdown

- Exports all required config and parameters for use by downstream tasks and modules.
- Enables safe, transparent customization and immediate repeatability (lab, demo, or test scenarios).

## Dependencies & Relationships

- Included by every major playbook, referenced by most templates.
- User can override at runtime or by file edit for scenario-specific configuration.

## Output/Result
