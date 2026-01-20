# ceph_deployments.yml Documentation

## Purpose & Overview

Deploys a suite of Ceph and Kubernetes configuration files to all control plane nodes in the cluster. It renders key Jinja2 templates into YAML manifests for storage, virtualization, and cluster configuration needed for a Ceph-backed Kubernetes environment.

## When to Use

Run this playbook after control plane nodes are provisioned but before initial cluster storage, toolbox, and compute workloads are deployed. It ensures all supporting manifests are available in each control plane host's home directory.

## Key Variables & Inputs

- `username`: The user under which the configs will be written.
- `groups['controlplane']`: Inventory group of target nodes.
- No other required variables; customization is generally achieved by editing referenced Jinja2 templates.

## Task Breakdown

For each control plane node, the playbook:

- Renders and distributes the following configuration files using Ansible's template module:
  - **cluster.yaml**: From cluster.j2, defines the Ceph Cluster.
  - **toolbox.yaml**: From toolbox.j2, administrative pod configuration for Ceph management.
  - **filesystem.yaml**: From filesystem.j2, CephFileSystem resource definition.
  - **storageclass.yaml**: From storageclass.j2, K8s StorageClass for Ceph-backed storage.
  - **pvc.yaml**: From pvc.j2, K8s PersistentVolumeClaim for dynamic storage.
  - **vm.yaml**: From vm.j2, sample virtual machine resource.
- Each file is created in `~/` of the delegated node with mode 0644 and correct ownership.
- Uses `become` for privilege escalation as needed.

## Dependencies & Relationships

- **Templates Used:** cluster.j2, toolbox.j2, filesystem.j2, storageclass.j2, pvc.j2, vm.j2 (all in ocne/templates/).
- **Runs on:** Every host in the Ansible inventory group `controlplane`; user defined by `username` variable.
- Typically invoked after the infrastructure is deployed (e.g., after build.yml).

## Output/Result

- Each control plane node receives a full set of Ceph and K8s manifest YAMLs needed for further deployment and management.
- These files enable the subsequent setup and orchestration of persistent, Ceph-backed Kubernetes storage services.

## Example Run

```bash
ansible-playbook ceph_deployments.yml -e "username=opc"
