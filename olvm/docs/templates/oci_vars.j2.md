# oci_vars.j2 Documentation

## Purpose & Overview

Jinja2 template to emit a YAML variable file with OCI resource references (compartment, VCN, subnet/subnet domain) for all downstream automation tasks and templates in OLVM.

## When to Use

Render as oci_vars.yml for use with template-driven resource creation, host configuration, and cluster automation pipelines where up-to-date cloud IDs are required for resource lookups or teardown.

## Key Variables & Inputs

- `my_compartment_id`, `my_vcn_id`, `my_subnet_id`, `my_subnet_domain_name`
- Driven by previous provisioning/tasks; injected at runtime by parent playbooks.

## Task Breakdown

- Outputs YAML mapping of current cloud state for robust automation by downstream roles.
- No logic, just substitution.

## Dependencies & Relationships

- Required by playbooks: build.yml, create_instance.yml, any task creating or cleaning up cloud resources.
- Maintains environment parity and enables Infrastructure as Code repeatability.

## Output/Result
