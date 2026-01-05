# ovirt_import_template.yml Documentation

## Purpose & Overview

Automates importing external OVA or template images into an oVirt/Oracle Linux Virtualization Manager lab environment, providing a consistent “golden image” base for VM deployment, student labs, or proof-of-concept setups.

## When to Use

Run when bringing in new templates, updating base images, migrating between clouds/platforms, or resetting cluster training instances.

## Key Variables & Inputs

- Template/OVA URL, cluster/host/engine information
- Storage domain and connection details from surrounding environment

## Task Breakdown

- Uses Ansible and oVirt modules to import or register an OVA/template to the local storage domain.
- Validates and reports on import status, outputting result details for lab audit or troubleshooting.

## Dependencies & Relationships

- Precedes (and is required by) ovirt_create_vm_from_template.yml for VM instantiation.
- Consumed by create_instance.yml as part of full cluster workflows.

## Output/Result
