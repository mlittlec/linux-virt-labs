# create_hostfile_secondary_nic.yml Documentation

## Purpose & Overview

Generates and manages a host file mapping secondary NICs for KVM and engine nodes in the Oracle Linux Virtualization Manager lab, ensuring correct network and connectivity setup in clustered environments.

## When to Use

Run after instances and secondary network interfaces are provisioned, to create a hosts file with static mappings and support cluster-wide communication or storage/management segmentation.

## Key Variables & Inputs

- Requires up-to-date inventory, host facts, and network provisioning.
- Uses Ansible facts to resolve hostnames and secondary NIC details.

## Task Breakdown

- Gathers/subscribes necessary facts from engine and KVM hosts.
- Writes/updates a host file (likely /etc/hosts or a designated path) to match secondary NICs to host aliases or required IP addressing.

## Dependencies & Relationships

- Usually run as a step in network finalization, right after secondary NIC and static IP assignment playbooks.
- Must be in sync with running instance state and up-to-date facts.

## Output/Result
