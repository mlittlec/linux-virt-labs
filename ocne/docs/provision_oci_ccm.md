# provision_oci_ccm.yml Documentation

## Purpose & Overview

Configures and deploys the Oracle Cloud Infrastructure Cloud Controller Manager (OCI CCM) module on the cluster, including the required firewall ingress/egress rules for K8s cloud integration and load balancer services.

## When to Use

Use this playbook:

- After a successful Oracle CNE Kubernetes install when you want the cluster to have full OCI-native cloud integration—especially for Service Type=LoadBalancer and automatic networking.
- Any time you want to be sure CCM is installed and functional, such as during lab, CI, or self-managed cluster builds.

## Key Variables & Inputs

- `groups['controlplane']`, `groups['worker']`: Hosts needing firewall rule updates.
- `oci_vars.yml`: Must be loaded with current OCI config/OCIDs.
- `username`: User context for provisioning and module installation.
- `ocne_provision.stat.exists`: Controls conditional deployment.
- Cloud OCID variables: `my_compartment_id`, `my_vcn_id`, `my_subnet_id`.
- `debug_enabled`: (Optional) Enables detailed step-by-step printout.

## Task Breakdown

- **Firewall Configuration:** Opens port 10256/tcp for oci-ccm, and 30000-32767/tcp for LB services (on all control plane and worker nodes).
- **OCI Credential Ingestion:** Reads the latest OCID information from oci_vars.yml.
- **Istio/Cluster Diagnostic Prints:** Optional info printout aids troubleshooting.
- **OCI CCM Module Creation/Install:**
  - Uses `olcnectl` to create and install the oci-ccm K8s module, leveraging instance principals and current K8s context.
- **Debug:** Prints standard install outputs stepwise when requested.

## Dependencies & Relationships

- Heavily depends on all prior OCI/Oracle CNE provisioning and network/resource state.
- Must be executed as the user/operator who owns the cluster context.
- Sibling step to other feature modules (Istio, Ceph, FSS, Kubectl).

## Output/Result

- A running OCI CCM deployment backing Service Type=LoadBalancer, advanced cloud integration, and dynamic networking in the K8s cluster.

## Example Run

```bash
ansible-playbook provision_oci_ccm.yml
