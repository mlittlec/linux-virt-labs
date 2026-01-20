# ovirt_create_vm_from_template.yml Documentation

## Purpose & Overview

Automates creation of new VMs from existing OVA or template images in Oracle Linux Virtualization Manager, enabling rapid scaling of lab, test, or DR workloads with repeatable configuration.

## When to Use

Run when needing to deploy new VMs based on golden images/templates for standardized lab, student, or integration environments.

## Key Variables & Inputs

- Template name, cluster/host, and VM inventory determined by upstream playbook context.
- Storage/CD-ROM and network settings inherited from environment and prior steps.

## Task Breakdown

- Uses Ansible's oVirt modules to create/import VMs from OVA/template.
- Customizes VM definition (CPU, storage, network) to match lab requirements.
- Reports status and any errors to orchestrator/CI logs for troubleshooting.

## Dependencies & Relationships

- Recipe is dependent on existing imported OVA/template (from ovirt_import_template.yml).
- Tightly coupled to cluster and storage domain state.

## Output/Result
