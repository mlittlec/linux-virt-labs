# lb_vars.j2 Template Documentation

## Purpose

Dynamically generates a YAML variable file with the OCI private load balancer primary IP address after it is provisioned by automation.

## How the Template is Used

- Rendered by create_lb.yml as the file lb_vars.yml.
- The resulting variable is referenced by playbooks and templates that require knowledge of the Load Balancer endpoint, including application profiles (e.g. Istio, quick/HA install, manual config).
- Ensures the runtime value is inserted exactly as provisioned in OCI.

## Key Variables & Substitutions

- `private_load_balancer_ip_addresses`: An array of IP objects as returned by the OCI module.
  - Only the first IP (primary) is extracted and set as `private_load_balancer_ip_addresses` in the output YAML.
- Jinja2 substitution is used to insert the live provisioned value.

## Rendered Example

```yaml
private_load_balancer_ip_addresses: 10.0.0.25
```

## Special Logic

- Extracts only the first element of the OCI return array.
- Used as a variable file; no additional looping or conditionals.

## Related Files

- Created by: create_lb.yml.
- Consumed by: quick/HA deployment playbooks, Istio/OCI annotation profiles, explain/config docs.
