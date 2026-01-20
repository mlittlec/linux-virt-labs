# oci_vars.j2 Documentation

## Purpose & Overview

Emits YAML config with core OCI resource IDs—compartment, VCN, subnet, and domain name—for Ansible-driven automation and template rendering.

## When to Use

Use this template to export current OCI resource IDs for use by other playbooks and templates that require orchestrated API access, security, or network context.

## Key Variables & Substitutions

- `my_compartment_id`: Target compartment OCID in OCI.
- `my_vcn_id`: VCN OCID.
- `my_subnet_id`: Subnet OCID.
- `my_subnet_domain_name`: Subnet's DNS-friendly name.
- All values are injected at runtime via Ansible variable system.

## Dependencies & Relationships

- Typically created by create_instance.yml or provisioning playbooks.
- Consumed by build.yml, block.yml, and security templates, ensuring all spawned resources are correctly referenced.
- Must be current and in sync with provisioned infrastructure for automated teardown or host/role updates.

## Output/Result

- YAML file with all major IDs ready to include in automation flows for accurate, robust, and repeatable lab setups.
