# ocir_vars.j2 Template Documentation

## Purpose

Exports registry (OCIR) metadata—repository ID, name, and namespace—for the use of automation and downstream playbooks configuring container images, access, and K8s integration.

## How the Template is Used

- Rendered by create_ocir.yml to create ocir_vars.yml.
- This YAML is then included and referenced wherever registry configuration, access, or deployments are needed across the environment (cluster bootstrap, image pulls, etc).

## Key Variables & Substitutions

- `ocir_id`: OCID of the OCI container registry.
- `ocir_name`: Name of the provisioned OCI registry.
- `ocir_namespace`: The namespace assigned in OCIR.
- All are Jinja2 variables set at runtime based on OCI API results.

## Rendered Example

```yaml
ocir_id: ocid1.containerrepo.oc1..abcd
ocir_name: demo/oraclelinux-cloud
ocir_namespace: demo123
```

## Special Logic

- Static key emission per invocation/call—no conditionals, no loops.
- Each value fully customized by current OCI registry state.

## Related Files

- Written by: create_ocir.yml.
- Included by: any downstream playbook or script requiring OCI registry info.
