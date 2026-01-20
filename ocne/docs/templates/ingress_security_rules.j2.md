# ingress_security_rules.j2 Template Documentation

## Purpose

Defines the default and optional ingress security rules for instances in OCI, consumed by Ansible `include_vars` for programmatic firewall/network list creation.

## How the Template is Used

- Rendered by playbooks such as create_instance.yml as a dynamic YAML variable file (`instance_ingress_security_rules`).
- Consumed by OCI networking modules to configure security list ingress rules for all VMs in the cluster.

## Key Variables & Substitutions

- Templated on the Ansible variable `use_ingress_lb`:
  - If true, opens HTTP (80/TCP) and HTTPS (443/TCP) from all sources.
  - Always allows SSH (22/TCP), local VCN traffic, and certain ICMP codes.
- Static entries for internal and cloud-classic networking requirements.

## Rendered Example (when use_ingress_lb is true)

```yaml
instance_ingress_security_rules:
  - source: "0.0.0.0/0"
    protocol: 6
    tcp_options:
      destination_port_range:
        max: 22
        min: 22
  - source: "0.0.0.0/0"
    protocol: 6
    tcp_options:
      destination_port_range:
        max: 80
        min: 80
  - source: "0.0.0.0/0"
    protocol: 6
    tcp_options:
      destination_port_range:
        max: 443
        min: 443
  - source: "0.0.0.0/0"
    protocol: 1
    icmp_options:
      type: 3
      code: 4
  - source: "10.0.0.0/24"
    protocol: 6
    tcp_options:
      destination_port_range:
        min: 1
        max: 65535
  - source: "10.0.0.0/24"
    protocol: 17
    udp_options:
      destination_port_range:
        min: 1
        max: 65535
  - source: "10.0.0.0/24"
    protocol: 1
    icmp_options:
      type: 3
```

## Special Logic

- Uses Jinja2 `{% if use_ingress_lb %}` to conditionally expand the security list.
- Supports both public and local VCN communication.
- Fully YAML, no further logic or looping.

## Related Files

- Rendered/consumed by: create_instance.yml or other network provisioning playbooks.
- Used with: egress_security_rules.j2 for comprehensive firewalling.
