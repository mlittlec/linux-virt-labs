# egress_security_rules.j2 Documentation

## Purpose & Overview

YAML/Jinja2 template for OCI instance security lists, allowing unrestricted TCP egress from all lab hosts (“allow all” rule).

## When to Use

Include wherever automated security policy/gen is needed for outbound network access (e.g., VMs, proxy, or automation endpoints in lab).

## Key Variables & Inputs

- Static—no variables or dynamic logic.
- Exposes `instance_egress_security_rules` as a list (all TCP traffic to any destination).

## Task Breakdown

- Standardizes “allow all” lab egress in security list YAML files.
- Used in automation for hands-on, demo, or multi-node labs.

## Dependencies & Relationships

- Primarily included or imported by main lab provisioning workflows.
- Should be cross-linked to ingress_security_rules.j2 for full OCI policy config.

## Output/Result

- Hosts will be granted unrestricted TCP egress during provisioning.
