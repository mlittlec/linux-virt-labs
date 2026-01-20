# provision_pah.yml Documentation

## Purpose & Overview

Deploys and configures Oracle Linux Private Automation Hub (PAH) on a dedicated host for managing collections, content, and roles in Oracle Linux Automation Manager lab and hybrid infrastructures.

## When to Use

Run to enable a lab-internal automation hub for distributing approved or custom content, especially when working in teams or disconnected environments.

## Key Variables & Inputs

- Hosts: ol-pah
- OS version detection, user/group variables for PAH inventory/ownership.

## Task Breakdown

- Installs/corrects EPEL, disables obsolete repositories, enables Oracle Linux Automation Manager PAH repo.
- Installs PAH installer, copies playbooks and templated inventory (hosts.j2), and runs single-node installation.
- Sets up ansible environment files (single_node), does everything with correct user ownership and permission.
- Runs final PAH installer playbook using ansible, printing results for diagnostics.

## Dependencies & Relationships

- Consumed by create_instance.yml and devops/project-level topology.
- Requires a host named "ol-pah" and user credential handling.

## Output/Result
