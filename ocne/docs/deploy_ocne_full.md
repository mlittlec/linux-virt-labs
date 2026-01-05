# deploy_ocne_full.yml Documentation

## Purpose & Overview

Prepares and deploys a complete, multi-node Oracle Cloud Native Environment (Oracle CNE) cluster spanning operator, control plane, and worker hosts. Automates system prep (SELinux, OS config, networking), time sync, repo setup, swap disablement, core package installation, firewall/kernel setup, Oracle CNE agent/api-server enablement, and certificate management, followed by orchestrating the full Oracle CNE install (platform CLI, ccm, Istio, storage, etc).

## When to Use

Use this playbook:

- When deploying a production-like or well-segmented Oracle CNE Kubernetes environment with Operator, Control-Plane, and Worker nodes on OCI.
- For lab/testing where a realistic, full-topology configuration is required.

## Key Variables & Inputs

- Host group memberships: `operator`, `controlplane`, `worker`
- Environment vars and toggles in `default_vars.yml`, `oci_vars.yml`
- `ocne_registry_location`: Container registry with Oracle CNE images.
- Fact variables passed from previous provisioning (IDs, domains, feature toggles).

## Task Breakdown

- **System Pre-Install:** SELinux config for iptables, repo configuration (OL8/OL9), Chrony installation, swap disablement—on all relevant hosts.
- **Debug/Info Tasks:** Prints important group and network config when `debug_enabled`.
- **OCI CLI Install:** OS-dependent OCI CLI and tool installation.
- **Firewall/Networking:** Firewall rules and kernel module (br_netfilter) setup for all roles, tailored to Oracle CNE needs.
- **Package Install:** Operator, Control-Plane, and Worker receive relevant Oracle CNE packages and agent tap.
- **Service/Group Setup:** Oracle CNE API and agent service enablement and user group adjustments.
- **Certificate Lifecycle:** Extensive certificate/gen/copy for Kubernetes and webhooks.
- **Bootstrap:** Starts API server/agent with proper security context; avoids reruns via .ocne-provisioned sentinel.
- **Include/Task Orchestration:** Invokes further tasks for provisioning, CCM, Istio, Ceph, FSS, Kubectl, based on cluster state and toggles.

## Dependencies & Relationships

- Highly cross-linked: includes/invokes provision_ocne.yml, ceph_deployments.yml, fss_deployments.yml, provision_istio.yml, preconfig_oci_ccm.yml, etc.
- Requires: Oracle CNE packages, olcnectl, OCI CLI, compatible Oracle Linux hosts, correct group/host membership.
- Performs actions as root or via `username` from vars.

## Output/Result

- A fully initialized multi-node Oracle CNE cluster.
- All agents, services, and pre-requisites running with TLS/certificates in place.
- Optionally installed CCM, Istio, FSS, and supporting add-ons.

## Example Run

```bash
ansible-playbook deploy_ocne_full.yml
