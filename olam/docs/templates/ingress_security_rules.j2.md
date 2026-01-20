# ingress_security_rules.j2 Documentation

## Purpose & Overview

Feature-conditional Jinja2 YAML template for generating ingress network/security rules in Oracle Linux Automation Manager labs. Always opens SSH, and conditionally includes rules for FreeIPA, PAH, cluster ports, and other core services based on feature flags.

## When to Use

Render this template to define security policies for cloud-played VMs in Oracle Linux Automation Manager workflows. Used at provisioning time or when modifying network policies/scenarios.

## Key Variables & Inputs

- Feature flags: use_freeipa, use_olam_pah, add_pah_ports, add_cluster_ports, add_haproxy_ports
- All rules are controlled by scenario toggles; SSH (22/TCP) is always opened

## Task Breakdown

- Emits list “instance_ingress_security_rules” for use in OCI/Ansible
- Includes only relevant rules for enabled services and demos, minimizing unnecessary attack surface

## Dependencies & Relationships

- Rendered/imported by create_instance.yml, cluster build flows, and network setup tasks
- Should always be paired with egress_security_rules.j2 for balanced security policy management

## Output/Result

- YAML block of ingress rules; imported into OCI security lists or lab firewalls
