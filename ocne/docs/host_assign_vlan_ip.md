# host_assign_vlan_ip.yml Documentation

## Purpose & Overview

Assigns static VLAN IPv4 addresses to all cluster nodes (Operator, Control-Plane, Worker) in OCI using the nmcli tool. Ensures network connectivity and complete mesh trust (SSH known_hosts) for subsequent cluster operations that rely on predictable VLAN addressing.

## When to Use

Use this playbook:

- After VLAN resources are provisioned and before cluster bootstrapping or role assignment.
- To ensure all hosts have deterministic, unambiguous VLAN IPs and that SSH trust is pre-loaded.

## Key Variables & Inputs

- `ansible_ens5`: Host interface fact used for addressing and validation.
- `group_names`: Used to assign correct addresses per host role.
- `groups['controlplane']`, `groups['worker']`, `groups['all']`: Define node pools for assignment loops.
- `username`: Used for SSH known_hosts population step.
- `debug_enabled`: Enables per-node debug output.

## Task Breakdown

1. **VLAN IP Assignment:**
   - Operator: 10.0.12.100/24
   - Control-plane: 10.0.12.1XX/24 (XX based on index in group)
   - Worker: 10.0.12.2XX/24 (XX based on index in group)
   - Uses nmcli for connection management on interface ens5.
2. **Gather facts:** Refreshes Ansible facts after changes.
3. **Debug (optional):** Outputs hostvars for troubleshooting.
4. **SSH Trust Prep:**
   - Uses ssh-keyscan to pre-accept fingerprints for all node-to-node connections (when cluster size >=2).

## Dependencies & Relationships

- Requires nmcli and working network interface on each node.
- Commonly used post-VLAN creation from create_vlan.yml and pre-setup before Oracle CNE or K8s installs.

## Output/Result

- Each cluster node receives a role-specific VLAN IP and has all other nodes' fingerprints trusted for SSH, minimizing connection errors in clustered playbooks.

## Example Run

```bash
ansible-playbook host_assign_vlan_ip.yml
