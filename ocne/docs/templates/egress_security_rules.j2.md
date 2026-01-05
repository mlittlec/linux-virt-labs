# egress_security_rules.j2 Template Documentation

## Purpose

Defines default egress security rules for cloud instance networking in OCI. Used as a variables file for dynamic security rule creation by Ansible playbooks during provisioning.

## How the Template is Used

- Rendered and included by playbooks such as create_instance.yml.
- Loaded via Ansible's include_vars to produce a variable named `instance_egress_security_rules`.
- Used as input to OCI network/security module to set up firewall rules for outbound traffic for new instances/subnets.

## Key Variables & Substitutions

- No Jinja2 variables or substitutions; a static set of YAML rules is rendered.

## Rendered Example

```yaml
instance_egress_security_rules:
  - destination: "0.0.0.0/0"
    protocol: 6
  - destination: "10.0.0.0/24"
    protocol: 17
```

## Special Logic

- The list provides two default egress rules:
  - Allow all TCP outbound to the internet (destination 0.0.0.0/0, protocol 6).
  - Allow all UDP outbound to 10.0.0.0/24 (protocol 17), mirroring default cloud security best practice.
- No dynamic templating.

## Related Files

- Rendered by: create_instance.yml and potentially other infrastructure playbooks managing network security lists.
- Works with ingress_security_rules.j2 as a complementary pair for full firewall configuration.
