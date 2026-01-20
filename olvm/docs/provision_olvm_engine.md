# provision_olvm_engine.yml Documentation

## Purpose & Overview

Deploys and configures the Oracle Linux Virtualization Manager (oVirt) Engine on a dedicated VM/host, prepping the management and control plane for KVM virtualization clusters.

## When to Use

Run to bring up a new Oracle Linux Virtualization Manager control/engine, when rebuilding the engine, or onboarding new management topology in the lab/test infrastructure.

## Key Variables & Inputs

- Host: engine group
- Requires all engine and base variables set (default_vars.yml, cluster info, engine image/version)

## Task Breakdown

- Installs the engine RPMs/OVA/templates as required.
- Handles engine user setup, initial configuration, connectivity validation, and base service enablement/startup.
- Exposes configuration (address, host, port) for subsequent playbooks and inventory.

## Dependencies & Relationships

- Consumed by orchestrators and subsequent cluster deployments.
- Related doc: provision_olvm_engine_publickey.yml for trust setup.

## Output/Result
