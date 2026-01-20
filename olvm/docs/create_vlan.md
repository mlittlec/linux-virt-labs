# create_vlan.yml Documentation

## Purpose & Overview

Automates the creation of an OCI VLAN (layer 2 network) and associated security group for KVM and engine hosts in Oracle Linux Virtualization Manager labs—enables advanced networking topologies, isolation, and segmentation.

## When to Use

Run after VCN and subnets are provisioned, before attaching secondary VLANs or overlaying virtualized environments on lab VMs or clouds.

## Key Variables & Inputs

- Networking resource IDs (compartment, VCN, route table), VLAN CIDR blocks, plus OCI config/profile context.
- Security rules are set per default lab communication needs.

## Task Breakdown

- Creates an OCI NSG (network security group) for the VLAN, writes its OCID to a state file for orchestration tracking.
- Adds egress (all) and restricted ingress (10.0.0.0/16) rules to the VLAN NSG.
- Provisions a VLAN object including NSG, assigns correct tag, attaches to public route table, and writes resource OCID to state file.

## Dependencies & Relationships

- Requires VCNs, base subnets, and routing already be provisioned (by create_instance.yml or equivalent).
- Upstream dependencies: block.yml, build.yml, etc.
- Written OCIDs are used by future playbooks for topology expansion and deletion.

## Output/Result
