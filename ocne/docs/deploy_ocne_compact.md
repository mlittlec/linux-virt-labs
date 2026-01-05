# deploy_ocne_compact.yml Documentation

## Purpose & Overview
Prepares the operator node for an Oracle Cloud Native Environment (Oracle CNE) compact deployment in OCI. Handles OS-level prerequisites, configures Oracle CNE repositories, networking, time sync, disables swap, adds kernel and firewall requirements, installs required Oracle CNE components, bootstraps security certificates, and launches Oracle CNE control-plane services.

## When to Use

Use this playbook:

- As the first step for deploying a single-node (operator-based) Oracle CNE/Kubernetes.
- To set up a working Oracle CNE control plane in a minimal or lab installation scenario.

## Key Variables & Inputs

- `ocne_registry_location`: Container registry for Oracle CNE images.
- `control_plane_nodes`: Comma-separated list of operator hostnames.
- `username`: OS user for privileged actions (from default_vars.yml).
- Other environment/host variables as from included default_vars.yml

## Task Breakdown

- **Show Nodes/Debug:** Optionally prints target node list.
- **Configure Repositories:** Sets up Oracle CNE YUM repos for OL8 or OL9.
- **Install Chrony:** Installs and enables Chrony for time synchronization.
- **Disable Swap:** Disables swap both immediately and in /etc/fstab.
- **Configure Firewall:** Opens required TCP/UDP ports for Oracle CNE/cluster operation.
- **Add Firewall Interface:** Adds trusted CNI interface for container networking.
- **Kernel Modules:** Loads and persists required kernel modules.
- **Install OLCNE Packages:** Installs main Oracle CNE tools and agents.
- **User/Group Setup:** Adds the operator user to the `olcne` group and resets SSH connections.
- **Service Setup:** Manages Oracle CNE API server and agent services.
- **Certificate Management:** Generates, distributes, and installs required X.509 certificates using `olcnectl`.
- **Bootstrap Services:** Starts Oracle CNE API server and agent with required arguments and keys.

## Dependencies & Relationships

- Used after network and compute provisioning is done (see create_instance.yml).
- Invokes other task files (ol8_repo_config.yml, ol9_repo_config.yml).
- Strongly coupled with operator node details as provisioned earlier.
- Oracle CNE repo setup and `olcnectl` must be available on the target host.

## Output/Result

- An operator node with all requisite packages, services, security artifacts, and configurations to serve as Oracle CNE/K8s control plane (or standalone node).

## Example Run

```bash
ansible-playbook deploy_ocne_compact.yml --limit operator
