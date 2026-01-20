# ovirt_add_logical_network.yml Documentation

## Purpose & Overview

Automates adding a logical network to an oVirt/Oracle Linux Virtualization Manager cluster, enabling segmented traffic, improved security, and sophisticated lab/demo topologies.

## When to Use

Run when you need additional logical networks (beyond default) to support workload separation, HA, custom routing, or special-purpose VMs.

## Key Variables & Inputs

- Network definition, VLAN/segment IDs, cluster/host info, all determined by lab scenario and upstream inventory.

## Task Breakdown

- Uses Ansible oVirt modules to add and configure the desired logical network in the Oracle Linux Virtualization Manager cluster.
- Ensures new networks are applied to the right clusters and attached to the correct hosts.
- Handles VLAN tags, MTU, and custom lab segmentation options.

## Dependencies & Relationships

- Called after hosts and the Oracle Linux Virtualization Manager engine/cluster are online.
- Must synchronize with physical/VM NIC configuration for lab scenarios.

## Output/Result
