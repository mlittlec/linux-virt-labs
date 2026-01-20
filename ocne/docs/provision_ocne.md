# provision_ocne.yml Documentation

## Purpose & Overview

Performs a manual installation of the Oracle Cloud Native Environment (Oracle CNE) Kubernetes platform. It leverages the `olcnectl` CLI to create the environment, initialize K8s modules (with optional LB support), validate, and install the cluster, then saves config.

## When to Use

Use this playbook:

- When automating Oracle CNE installs in "manual" mode (rather than quick/compact).
- When your cluster needs explicit differentiation of control plane layouts (single vs. multi, load balancer vs. not).
- As an orchestrated step in full OCI automation or for reproducible demo/lab environments.

## Key Variables & Inputs

- `operator_nodes`, `control_nodes`, `worker_nodes`: Node selection strings for API server, CP, and worker roles.
- `private_load_balancer_ip_addresses`: For clustered API endpoint in multi-CP, LB use cases.
- `ocne_registry_location`: Container registry source for K8s images.
- `use_lb`: Controls which config path is taken (LB vs. non-LB).
- `username`: The privilege user running `olcnectl` and provisioning.
- Cluster certificate references for secure install (optional).
- `debug_enabled`: Toggles diagnostic output.
- see default_vars.yml and deploy playbooks for more (many inputs inherited).

## Task Breakdown

- **Manual (Non-LB) Install Path:** Runs environment and K8s module create/validate/install with just the API/CP entries.
- **Manual LB Path:** Loads LB vars, runs `olcnectl` with `--load-balancer` and correct host/IP layout.
- **Debug (optional):** Prints install output result.
- **Provision Sentinel:** Touches `~/.ocne-provisioned` if successful.
- **Config Save:** Exports fresh Oracle CNE config for CLI/dev use.

## Dependencies & Relationships

- Requires ocne_registry_location, nodes, certs, and correct user context.
- Touches and uses ~user/ state to track provisioning.
- Sibling/alternative to deploy_ocne_* and create_instance.yml orchestrations.

## Output/Result

- Complete, validated, and installed Oracle CNE Kubernetes cluster (config saved).
- Clear, idempotent state via provision marker file for future automation safety.

## Example Run

```bash
ansible-playbook provision_ocne.yml
