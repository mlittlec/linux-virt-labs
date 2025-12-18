# oci_vars.j2 — Jinja2 Template for OCI Resource Variable Output

This template renders a YAML snippet containing essential OCI resource identifiers for use throughout the automation project.

## Purpose

- Serializes core environment variables for easy inclusion and referencing in downstream Ansible tasks and playbooks.
- Consolidates compartment, VCN, and subnet IDs and domain names into a single file.

## Input Variables

| Variable                | Description                         |
|-------------------------|-------------------------------------|
| my_compartment_id       | OCID of target compartment          |
| my_vcn_id               | OCID of Virtual Cloud Network       |
| my_subnet_id            | OCID of target subnet               |
| my_subnet_domain_name   | Domain name of provisioned subnet   |

## Main Output

A YAML file (e.g., `oci_vars.yml`) containing references to the constants above, which are then loaded where needed (e.g., with `include_vars`, or as direct file inclusion).

## Relationships

- **Rendered by:** `create_instance.yml` as part of early provisioning
- **Consumed by:** Playbooks like `deploy_ocne_oci.yml` and others that require networking or compartment context

## Example Output

```yaml
my_compartment_id: ocid1.compartment.oc1..exampleuniqueID
my_vcn_id: ocid1.vcn.oc1..exampleuniqueID
my_subnet_id: ocid1.subnet.oc1..exampleuniqueID
my_subnet_domain_name: subnet.example.oraclevcn.com

