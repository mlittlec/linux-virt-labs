# deploy_olam_olae.yml Documentation

## Purpose & Overview

Installs the core "ansible-core" package on Oracle Linux Automation Manager control nodes using dnf/yum, preparing the system for Oracle Linux Automation Engine (OLAE) workloads.

## When to Use
Use for standalone Oracle Linux Automation Engine deployments, baseline lab environments, or developer/test setups that require only Ansible Engine (not the full Oracle Linux Automation Manager suite).

## Key Variables & Inputs

- Hosts: control group
- default_vars.yml included

## Task Breakdown

- Installs ansible-core from OS repositories, with retry/delay for robustness.

## Dependencies & Relationships

- Can be included as an import in create_instance.yml, or run as a standalone Oracle Linux Automation Engine preparation.
- Required as a base step for any Oracle Linux Automation Engine environment where lab automation is desired.

## Output/Result
