# create_vlan.yml Documentation

## Purpose & Overview

Automates the creation of a VLAN in Oracle Cloud Infrastructure (OCI), configuring it within a target Virtual Cloud Network (VCN) and compartment, and applying a dedicated Network Security Group (NSG) with suitable ingress/egress rules.

## When to Use

Use this playbook when:

- VLAN-based isolation is needed for cluster resources or workloads within a Linux/Kubernetes OCI deployment.
- You want reproducible, API-driven setup of VLAN and its network security.

## Key Variables & Inputs

- `my_availability_domain`: OCI AD for the VLAN.
- `vlan_cidr_block`: Address range for the VLAN.
- `my_compartment_id`: OCI compartment OCID.
- `my_vcn_id`: The ID of the Virtual Cloud Network for VLAN/NSG association.
- `debug_enabled`: Enables detailed debug information if set.

## Task Breakdown

- **Create Network Security Group:** Using `oci_network_security_group`, provisions a new NSG scoped to the VLAN's VCN and compartment.
- **Set NSG Rules:** Adds both egress (all traffic to 0.0.0.0/0) and ingress (all from VCN CIDR) rules to the NSG via `oci_network_security_group_security_rule_actions`.
- **VLAN Creation:** Allocates the VLAN resource within the OCI AD, VCN, and compartment, attaching the new NSG.
- **Fact Propagation:** Stores IDs for chaining to future playbooks/actions.
- **Debug Output:** Optionally prints resultant VLAN and NSG state.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: NSG and VLAN modules.
- Relies on previously created VCN (from create_instance.yml), correct compartment/AD configuration, and optionally interacts with other network playbooks for further setup.

## Output/Result

- A new VLAN resource in OCI, with dedicated network security group and rules.
- Exposed variables (`my_vlan_id`, `my_nsg_id`) for use in provisioning compute instances, load balancers, etc.

## Example Run

```bash
ansible-playbook create_vlan.yml \
  -e "my_compartment_id=..." \
  -e "my_availability_domain=..." \
  -e "my_vcn_id=..." \
  -e "vlan_cidr_block=10.0.1.0/24"
