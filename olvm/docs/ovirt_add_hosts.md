# ovirt_add_hosts.yml Documentation

## Purpose & Overview

Automates the process of adding KVM hosts (hypervisors/compute nodes) to an oVirt/Oracle Linux Virtualization Manager cluster, forming the scalable base for VM workloads and distributed management.

## When to Use

Run as part of an Oracle Linux Virtualization Manager environment bootstrap, or when a new KVM host needs to be integrated into an existing cluster for scale-out, HA, or migration scenarios.

## Key Variables & Inputs

- Target cluster information, host credentials, and inventory determined upstream.
- Requires KVM hosts to be available and SSH-reachable.

## Task Breakdown

- Registers/validates each KVM host with the oVirt/Oracle Linux Virtualization Manager engine.
- Tests connectivity, gathers host state, and ensures the hypervisor is attached to the target cluster.
- Handles errors and reports any failed adds for troubleshooting.

## Dependencies & Relationships

- Consumed by create_instance.yml and cluster setup sequences.
- Ties in with all networking, VLAN, and security configuration plays/templates.

## Output/Result
