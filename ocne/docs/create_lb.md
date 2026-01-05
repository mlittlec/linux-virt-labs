# create_lb.yml Documentation

## Purpose & Overview

Automates provisioning and configuration of a private load balancer on Oracle Cloud Infrastructure (OCI), intended to provide front-end access (port 6443/TCP) to a dynamic pool of Kubernetes control plane nodes. The playbook registers backends, configures health checks, and sets appropriate facts for use by downstream tasks and users.

## When to Use

Use this playbook when:

- Deploying a highly available Kubernetes or Oracle Linux cluster in OCI.
- You want to automate the deployment of an OCI load balancer, backend registration, and config recording for subsequent steps or documentation.

## Key Variables & Inputs

- `my_compartment_id`: OCI compartment OCID.
- `my_subnet_id`: Target subnet for the load balancer.
- `groups['controlplane']`: Determines which hosts become load balancer backends.
- `empty_cp_nodes`: Used to optionally filter out empty/missing control planes (integer count).
- `debug_enabled`: Turns stepwise debug prints on/off.

## Task Breakdown

- **LB Provisioning:** Creates a private load balancer via `oci_loadbalancer_load_balancer`.
- **Backend Set & Listener:** Creates backend set with TCP/6443 health checks and a listener using `oci_loadbalancer_backend_set` and `oci_loadbalancer_listener`.
- **Control Plane Targeting:** Dynamically discovers real control plane nodes for backend pool registration.
- **Backend Registration:** Adds each control plane host to the load balancer on required port.
- **Config Export:** Writes connection details to `lb_vars.yml` for later consumption using Jinja2 template.
- **Debug Output:** Optionally prints all relevant addresses and state details for review.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: load balancer modules, backend set/listener management.
- Uses data and facts from create_instance.yml and related playbooks.
- **Templates Used:** lb_vars.j2 for writing finalized variables.
- Interacts with all hosts in `controlplane` group and expects facts/addresses to be set correctly by prior steps.

## Output/Result

- A private OCI load balancer with health-checked K8s API backends.
- Updated configuration file `lb_vars.yml` reflecting endpoints and credentials for next-stage tasks.

## Example Run

```bash
ansible-playbook create_lb.yml \
  -e "my_compartment_id=..." \
  -e "my_subnet_id=..."
