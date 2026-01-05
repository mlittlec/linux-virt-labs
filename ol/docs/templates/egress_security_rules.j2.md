# egress_security_rules.j2 Documentation

## Purpose & Overview

Defines the default egress security rules for OCI instances in YAML format for automation inclusion. Enables unrestricted TCP outbound traffic from managed hosts.

## When to Use

Use as an include_vars input anywhere security list configuration is needed in automation or bootstrapping of Oracle Linux infrastructure for OCI (cloud or on-prem scenarios).

## Key Variables & Substitutions

- Static YAML structure, no Jinja2 substitutions.
- Exposes:
  - `instance_egress_security_rules`:
    - destination: "0.0.0.0/0"  (all hosts)
    - protocol: 6 (TCP)

## Dependencies & Relationships

- Used together with ingress_security_rules.j2 for full network policy.
- Typically included or rendered in create_instance.yml or cloud/network setup plays.

## Output/Result
