# ingress_security_rules.j2 — Jinja2 Template for Security List Ingress Rules

This Jinja2 template generates a YAML variable file defining ingress (inbound) security rules for OCI Security Lists, tailored for SSH access, web traffic, and NFS/FSS use cases.

## Purpose

- Defines rules for which inbound connections are permitted to cloud instances.
- Supports optional activation of load balancer and FSS-specific ports via variables.

## Input Variables

| Variable         | Description                                       |
|------------------|---------------------------------------------------|
| `use_ingress_lb` | Enables HTTP/HTTPS (80/443) load balancer rules   |
| `use_fss`        | Enables FSS NFS/port rules                        |

## Logic

- Always allows inbound SSH (port 22, TCP) from anywhere.
- If `use_ingress_lb` is true, opens ports 80 and 443 for web load balancer traffic.
- If `use_fss` is true, adds multiple port/protocol combinations for NFS and FSS integration.

## Main Output

- `instance_ingress_security_rules`: List of dictionary entries, consumed by OCI network configuration tasks.

## Relationships

- **Rendered by:** `create_instance.yml` with the `template` module.
- **Consumed by:** Security list creation tasks when new subnets are provisioned.

## Example Output

```yaml
instance_ingress_security_rules:
  - source: "0.0.0.0/0"
    protocol: 6
    tcp_options:
      destination_port_range:
        max: 22
        min: 22
  # Ports 80/443 and NFS rules added when variables are true

