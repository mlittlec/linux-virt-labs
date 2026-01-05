# provision_kubectl.yml Documentation

## Purpose & Overview

Enables non-root access to the Kubernetes cluster for all control plane hosts by ensuring correct placement of kubectl configuration files and making kube CLI accessible from login shells.

## When to Use

Use this playbook:

- After initializing an Oracle CNE/Kubernetes cluster and confirming /etc/kubernetes/admin.conf exists.
- Any time you want to enable a user (default: operator) on all control plane machines to use `kubectl` without root privileges or sudo.

## Key Variables & Inputs

- `groups['controlplane']`: Nodes to receive kubeconfigs.
- `username`: Target OS user for kubectl privilege.
- `empty_cp_nodes`: Controls if some control plane nodes are skipped (zero/edge cases).

## Task Breakdown

- **.kube Directory:** Creates ~/.kube with proper permissions for each target user.
- **Copy Config:** Copies /etc/kubernetes/admin.conf to /home/<username>/.kube/config on every control plane node.
- **Export KUBECONFIG:** Appends correct export statement to the `.bashrc` for each user, enabling automatic kubectl context on shell login.

## Dependencies & Relationships

- Requires Ansible `become` for file creation/copying.
- Must be run after Oracle CNE/Kubernetes install and before administrative operations.
- Downstream playbooks and user sessions depend on a working ~/.kube/config.

## Output/Result

- On every control plane machine, the target user can run kubectl immediately after login, no sudo required.

## Example Run

```bash
ansible-playbook provision_kubectl.yml
