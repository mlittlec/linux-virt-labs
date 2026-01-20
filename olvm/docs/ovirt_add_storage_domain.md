# ovirt_add_storage_domain.yml Documentation

## Purpose & Overview

Automates adding new storage domains to the oVirt/Oracle Linux Virtualization Manager cluster—enabling shared storage for VM provisioning, HA, and advanced disk management scenarios.

## When to Use

Run when expanding the storage pool, onboarding new applications, or setting up D/R, backup, or fast scaling capabilities in the virtual lab.

## Key Variables & Inputs

- Storage domain configuration (type, backend, connection info)
- Cluster/engine credentials and working infrastructure state

## Task Breakdown

- Uses Ansible's oVirt modules to define and add storage domains.
- Handles config, host assignment, and reporting details.
- Validates success and outputs errors/warnings for failed storage registration.

## Dependencies & Relationships

- Consumed by create_instance.yml and cluster orchestration.
- Dependencies on Oracle Linux Virtualization Manager/engine, physical or cloud-based storage, and previous block provisioning.

## Output/Result
