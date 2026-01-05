# create_instance.yml Documentation

## Purpose & Overview

Orchestrates end-to-end cloud and networking provisioning for an Oracle Linux Virtualization Manager test/lab cluster. Launches engine/kvm VMs, configures primary/subnet/VLAN networking and storage, syncs facts/state, and manages host inventories.

## When to Use

Run to fully build, scale, or reset a multi-VM Oracle Linux Virtualization Manager cluster/lab in OCI, with robust network and storage integration.

## Key Variables & Inputs

- All credential, AD, VCN/network, and image vars (default_vars.yml)
- engine/kvm role, SSH credentials, block count, subnet/VLAN config

## Task Breakdown

- Gathers/sets facts on OCI/region/compartment, prepares .ansible-state ID tracking
- Provisions VCN, routing, subnets, security lists, images, and computes
- Loops over each instance—configuring public/private NICs, storage, VLANs, and facts
- Updates both in-memory and ini-style host inventories for cluster readiness

## Dependencies & Relationships

- Calls build.yml, create_block_storage.yml, and imports cluster/host prep tasks
- Used as main host/nodeset orchestration by bootstrapping scripts and teardown tasks

## Output/Result

- Full OLVM lab with engine and kvm hosts, all required secondary networks/storage, and idempotent state tracking for dev/test/CI/CD.
