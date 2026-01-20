# fss_deployments.yml Documentation

## Purpose & Overview

Generates and deploys Oracle Cloud Infrastructure File Storage Service (OCI FSS)-backed PersistentVolume (PV), PersistentVolumeClaim (PVC), and test pod YAML manifests to all control plane nodes. It enables Kubernetes workloads to consume OCI FSS storage for persistent data.

## When to Use

Use this playbook:

- After OCI FSS resources are provisioned and fss_vars.yml is generated.
- To enable storage integration between Kubernetes and OCI FSS via NFS for test or production pods.

## Key Variables & Inputs

- `fss_vars.yml`: Variable file that provides FSS NFS export path, IP addresses, and required storage info.
- `groups['controlplane']`: Inventory group that designates nodes to receive the YAMLs.
- `username`: OS user context for file deployment and permissions.

## Task Breakdown

- **Variable Load:** Loads all OCI FSS configuration (paths, IPs) from fss_vars.yml.
- **Manifests Creation:** 
  - Renders fss-pv.yaml from fss_pv.j2 (PersistentVolume config).
  - Renders fss-pvc.yaml from fss_pvc.j2 (PersistentVolumeClaim for consumption).
  - Renders fss-pod.yaml from fss_pod.j2 (demo/test pod mounting the PVC).
- **Deployment:** Each manifest is placed in the home directory of each control plane host, with correct permissions.

## Dependencies & Relationships

- Depends on prior completion of create_fss.yml (and fss_vars.yml population).
- Templated using fss_pv.j2, fss_pvc.j2, fss_pod.j2 from ocne/templates/.
- Used from main cluster install or post-provisioning as add-on to enable apps to use NFS storage.

## Output/Result

- Each control plane node receives a full set of PV, PVC, and pod manifests ready for `kubectl apply`.
- Enables seamless Kubernetes-to-OCI-FSS storage integration.

## Example Run

```bash
ansible-playbook fss_deployments.yml
