# create_ocir.yml Documentation

## Purpose & Overview

Automates the creation of a private OCI Container Registry (OCIR) repository for secure Docker image storage and deployment. The playbook provisions a repository, fetches/exposes its key identifiers (ID, name, namespace), and writes them to a structured variable file for downstream use.

## When to Use

Use this playbook when:

- You need a private Docker image repository to support custom container builds, CI/CD, or image pulls within your Oracle Linux or Kubernetes lab environments.
- Integrating custom applications or distributing lab images in a test or production cluster.

## Key Variables & Inputs

- `my_compartment_id`: OCI compartment OCID.
- `debug_enabled`: Enables stepwise output and debugging information.

## Task Breakdown

- **Repository Provisioning:** Creates a container repo named `demo/oraclelinux-cloud` (private by default) in the specified compartment.
- **Debug Output (Optional):** Prints resulting repo details when enabled.
- **Fact/Var Collection:** Gathers the OCIR repo's unique ID, name, and namespace for cluster authentication.
- **Config Export:** Writes all retrieved registry information (~ocir_vars.yml) for consumption by further configuration or install steps.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: `oci_artifacts_container_repository`.
- Relies on variables for compartment selection.
- **Templates Used:** ocir_vars.j2 for output variables file.
- Used as part of create_instance.yml deployment or as a standalone prereq for enabling private registry features.

## Output/Result

- New OCIR repository suitable for private Docker image hosting.
- Variable file `ocir_vars.yml` that supplies registry information for consumption by other playbooks or user tooling.

## Example Run

```bash
ansible-playbook create_ocir.yml \
  -e "my_compartment_id=..."
