# egress_security_rules.j2 — Jinja2 Template for Security List Egress Rules

This Jinja2 template generates a YAML variable file defining egress (outbound) security rules used in OCI network Security Lists for new compute and application instances.

## Purpose

- Outputs a structure (`instance_egress_security_rules`) used to configure which outbound ports and protocols are permitted on each instance subnet.
- Used to restrict or allow traffic, controlling NFS, general access, and K8s/FSS integration.

## Input Variables

| Variable     | Description                                |
|--------------|--------------------------------------------|
| `use_fss`    | Enables additional FSS traffic rules       |

## Logic

- By default, allows outgoing TCP traffic to anywhere (0.0.0.0/0).
- If `use_fss` is true, adds dynamic rules for NFS ports/protocols used by OCI FSS and Kubernetes workloads.

## Main Output

- `instance_egress_security_rules`: List of egress security rule dictionaries, ready for inclusion with OCI Ansible modules.

## Relationships

- **Rendered by:** `create_instance.yml` using `template` module.
- **Consumed by:** OCI security list tasks when creating subnets and firewall rules.

## Example Output

```yaml
instance_egress_security_rules:
  - destination: "0.0.0.0/0"
    protocol: 6
  - destination: "10.0.0.0/24"
    protocol: 6
    tcp_options:
      source_port_range:
        max: 111
        min: 111

