# preconfig_oci_ccm.yml Documentation

## Purpose & Overview

Prepares essential cloud environment configuration for Oracle Cloud Infrastructure Cloud Controller Manager (OCI CCM) integration by populating mandatory OCI IDs as bash environment variables.

## When to Use

Use this playbook:

- After OCI infrastructure (network, subnets, compartments) is provisioned and oci_vars.yml is available.
- Before installing/configuring any component that relies on these IDs as shell environment variables (OCI CCM daemon, scripts, cluster bootstrap).

## Key Variables & Inputs

- `oci_vars.yml`: Contains `my_compartment_id`, `my_vcn_id`, `my_subnet_id` from prior runs.
- `username`: User whose .bashrc gets updated.

## Task Breakdown

- **Load Variables:** Reads OCI IDs and other vars from oci_vars.yml.
- **Update .bashrc:** Appends `export` lines for compartment, VCN, and subnet OCIDs, making them available for new shell sessions and runtime scripts/services.

## Dependencies & Relationships

- Relies on oci_vars.yml, which should be generated via earlier provisioning.
- Often paired with further CCM or integration playbooks.

## Output/Result

- User’s ~/.bashrc includes key cloud OCIDs for CCM or operator scripts.

## Example Run

```bash
ansible-playbook preconfig_oci_ccm.yml
