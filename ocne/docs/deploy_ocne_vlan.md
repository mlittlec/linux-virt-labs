# deploy_ocne_vlan.yml Documentation

## Purpose & Overview

Automates the complete setup and deployment of an Oracle Cloud Native Environment (Oracle CNE) Kubernetes cluster on Oracle Cloud Infrastructure using VLAN networking. Covers advanced system prep, configures kernel/firewall, manages certificates, and installs Oracle CNE roles in a VLAN-context, supporting additional modules (CCM, Ceph, FSS, Kubectl).

## When to Use

Use this playbook:

- To build a realistic, multi-node Oracle CNE cluster where VLAN-based networking is required or preferred over default subnet networking.
- When wider cloud feature integration (internal LB, VRRP, custom CIDRs) is needed for complex lab, test, or DMZ-like scenarios.

## Key Variables & Inputs

- Inventory host groups: `operator`, `controlplane`, `worker`
- Host- and IP-specific variables derived from ansible_ens5 interface
- vars_files: `default_vars.yml`, `oci_vars.yml`
- Feature toggles: `use_int_lb`, `use_vlan_full`
- Various pre-set IDs, domain names, and settings from earlier provisioning plays

## Task Breakdown

1. **System Prep (All Hosts):**
   - SELinux local policy patch for iptables.
   - Oracle CNE YUM repo config for OL8/OL9.
   - Chrony NTP setup and enable.
   - Swap disablement and persistent config.
2. **OCNE Install (Operator):**
   - Writes bashrc profile with compartment IDs, locale.
   - Installs oci-cli, jq tools per Oracle Linux version.
   - Firewall rules for operator, internal/load balancers, VRRP as required.
   - Kernel module and cni0/trusted zone firewall for container networking.
   - Installs required Oracle CNE packages on all appropriate hosts/roles.
   - Service enablement for API server and agents via systemd.
   - User group updates and SSH connection resets.
3. **Certificate Handling:** 
   - Generates, distributes, and installs required TLS certificates for all nodes, webhooks, and CLI services.
4. **Manual Install (VLAN Mode):**
   - If single control plane: runs olcnectl module create/validate/install (manual mode).
   - If multi-control-plane/internal LB: handles LB virtual IP, VRRP firewall, module installs, and requires use_int_lb toggle.
5. **Post-Provisioning:**
6. 
   - Saves out Oracle CNE CLI config.
   - Invokes provisions for kubectl and other support tools.

## Dependencies & Relationships

- Requires multiple included playbooks (ol8_repo_config.yml, ol9_repo_config.yml, provision_kubectl.yml, etc).
- Highly dependent on variable population and host targeting from earlier provisioning.
- Applies to environments with VLAN-specific infrastructure and requirements.

## Output/Result

- Multi-node Oracle CNE cluster configured for VLAN operation, with all agents, roles, and services correctly enabled and exposed.
- Properly networked cloud-native/test lab with full certificate management and add-on readiness.

## Example Run

```bash
ansible-playbook deploy_ocne_vlan.yml
