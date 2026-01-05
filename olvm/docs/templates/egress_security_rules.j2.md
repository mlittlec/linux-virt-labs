# egress_security_rules.j2 Documentation

## Purpose & Overview

Jinja2/YAML template that defines default egress security rules for OCI VMs—allows unrestricted egress (all TCP traffic) for networking flexibility in Oracle Linux Virtualization Manager labs.

## When to Use

Include in Ansible security list configuration tasks to grant outgoing network access to all hosts by default (matches lab/testbed scenarios).

## Key Variables & Inputs

- Static structure; no variable substitutions.

## Task Breakdown

- Exposes the variable `instance_egress_security_rules`, listing:
  - Destination: "0.0.0.0/0" (all traffic)
  - Protocol: 6 (TCP)

## Dependencies & Relationships

- Referenced by create_instance.yml or any network/security automation playbook for VCN/lab setup.

## Output/Result
