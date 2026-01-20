# provision_istio.yml Documentation

## Purpose & Overview

Configures and installs the Istio service mesh on an Oracle CNE Kubernetes environment by templating the Istio load balancer configuration and invoking `olcnectl` to create and install the Istio module.

## When to Use

Use this playbook:

- To automate setup of the Istio ingress/service mesh in new or existing OCI-based Kubernetes clusters managed by Oracle CNE.
- After the core environment and cluster have been provisioned, and ocne_provision indicates completion.

## Key Variables & Inputs

- `username`: User context for creation/installation tasks.
- `ocne_provision.stat.exists`: Controls conditional execution (must be true for module creation).
- Other environment variables (from prior provisioning) and templates.

## Task Breakdown

- **Templating:** Renders istio-lb.yaml manifest from istio_lb.j2 with custom parameters.
- **Istio Module Creation:** Uses `olcnectl` to create the Istio module, referencing cluster and profile via CLI.
- **Debug (optional):** Prints stdout for troubleshooting or validation.
- **Module Installation:** Installs Istio on the targeted environment.
- **Debug (optional):** Prints output from installation step.

## Dependencies & Relationships

- Relies on istio_lb.j2 template for configuration.
- `olcnectl` must be installed and available under the selected user.
- Must run after a successful Oracle CNE cluster deployment (`ocne_provision.stat.exists`).

## Output/Result

- Instantiated and installed Istio service mesh within the Oracle CNE cluster, ready for ingress and advanced mesh features.

## Example Run

```bash
ansible-playbook provision_istio.yml
