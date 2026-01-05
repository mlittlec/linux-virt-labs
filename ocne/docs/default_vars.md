# default_vars.yml Documentation

## Purpose & Overview

Defines the default variables for all principal Ansible playbooks in the Oracle Cloud Native Environment (Oracle CNE) automation suite. Controls environment topology (instances, types, count), hardware sizing, options for enabling/disabling cloud features, debug toggles, and distribution settings.

## When to Use

- Included (via `vars_files`) by most playbooks to provide sane defaults for user convenience and repeatability.
- Edit as needed to adjust the size, features, or composition of your OCI-based Kubernetes/Oracle Linux cluster.

## Variables and Their Purposes

### Cluster Topology & Sizing

- **compute_instances**: Maps each node to a role (operator/controlplane/worker) and name.
- **os, os_version**: Linux flavor and version for instance provisioning.
- **instance_shape, instance_ocpus, instance_memory**: OCI instance shape, CPU, and RAM used for new VMs.
- **subnet_cidr_block**: The CIDR block assigned by default to the subnet for the cluster.

### VLAN & Networking Options

- **use_vlan**: Boolean toggle for VLAN provisioning and assignment.
- **vlan_cidr_block**: CIDR block for VLAN address space if enabled.

### User & Debug Options

- **username**: Target SSH/OS-level username for setup.
- **user_default_password**: Default password if user accounts require setting.
- **debug_enabled**: Enable verbose debug info and Ansible outputs.

### Oracle CNE/Cluster Type

- **ocne_type**: Controls which Oracle CNE deployment mode is used (quick/full/none).
- **use_ocne_full, use_vlan_full**: Additional toggles for full deployments or VLAN-everywhere.

### Optional Features

- **use_lb, use_int_lb, use_ingress_lb**: Enable/disable load balancer resources.
- **oci_ccm_bash, use_oci_ccm, use_istio**: CCM and Istio integration toggles.
- **use_fss**: Enable/disable Oracle File Storage Service provisioning.
- **add_ceph_block_storage, ceph_volume_size_in_gbs**: Enable/parametrize Ceph block storage creation.
- **add_ceph_deployments**: Enable Ceph configuration templating.
- **use_ocir**: Toggle for registry support.

### Oracle Linux Repos (for K8s/OLCNE)

- **ol8_enable_repo, ol8_disable_repo, ol9_enable_repo, ol9_disable_repo**: Names of Oracle Linux YUM repositories to enable/disable based on OS version.

### Advanced/Placeholder Vars

- **empty_cp_nodes, empty_wrk_nodes**: Used to modify/control pool sizing or test empty/edge cases.

## How to Customize

Edit this file before your run, or override its values at runtime with Ansible `-e` extra-vars:

```bash
ansible-playbook create_instance.yml -e "instance_memory=64 use_lb=true"
