# ingress_security_rules.j2 Documentation

## Purpose & Overview

Jinja2 YAML template listing default and feature-toggled ingress firewall/security rules for all VMs in Oracle Linux Virtualization Manager labs.

## When to Use

Use as include_vars or render target in security list configuration for initial node provisioning, or to expose HA, storage, and GUI ports in testbed and cluster builds.

## Key Variables & Inputs

- Feature toggles: opens additional ports for storage, HA, cluster, app, or remote desktop as required by scenario (variables in playbooks).
- Base rules always allow SSH (22/TCP).

## Task Breakdown

- Emits a structure under `instance_ingress_security_rules` with all baseline and optionally required ports (HTTPS, VNC, clustering, etc.).
- Fully YAML; no logic is executed here—behavior controlled externally.

## Dependencies & Relationships

- Consumed by create_instance.yml, create_vlan.yml, and any automation that manages network policy or OCI security lists.

## Output/Result
