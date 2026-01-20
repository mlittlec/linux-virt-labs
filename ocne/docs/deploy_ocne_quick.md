# deploy_ocne_quick.yml Documentation

## Purpose & Overview

Performs a streamlined, "quick" installation of Oracle Cloud Native Environment (Oracle CNE) using the olcnectl quick install mode. Brings up API server, control plane, and worker nodes in a single step (with or without LB), creates necessary cloud integration (CCM, Istio, FSS, Ceph), sets up OCI user/group config, and records provisioning details.

## When to Use

Use this playbook:

- To rapidly deploy Oracle CNE for lab, demo, or testing where speed and simplicity are the priority.
- When the number of control plane nodes or inventory determines the optimal quick vs. HA style install.

## Key Variables & Inputs

- `operator_node`, `control_plane_nodes`, `worker_nodes`: String lists of node hosts.
- `private_load_balancer_ip_addresses`: Used for LB integration if >1 control plane.
- `username`: User for privileged/cloud config on the operator node.
- Feature toggles (from default_vars.yml): `use_lb`, `use_oci_ccm`, `use_istio`, `use_fss`, `add_ceph_deployments`.
- Other included variable files for sub-tasks.

## Task Breakdown

- **Debug Info:** Print role/host groups as configured.
- **Repo & Package Setup:** Set up Oracle LCNE repos and install olcnectl.
- **Quick Provisioning:**
  - If single control plane: use olcnectl provision with just operator+control+worker.
  - If multiple control planes/LB: extends provision command to include `--load-balancer` with endpoints.
- **Provision Marking:** Touches ~/.ocne-provisioned to track completed installs.
- **Post-Install Steps:** Adds user to olcne group, resets SSH, saves Oracle CNE config, and invokes config/feature modules (CCM, Istio, Ceph, FSS, Kubectl).
- **Conditional Includes:** Uses Ansible logic to only run features as toggled or required.

## Dependencies & Relationships

- Requires supporting playbooks and files: lb_vars.yml, preconfig_oci_ccm.yml, provision_oci_ccm.yml, provision_istio.yml, ceph_deployments.yml, fss_deployments.yml, provision_kubectl.yml.
- User/group setup and cloud variables must be present.
- For load balancer use, expects variables (private_load_balancer_ip_addresses) from earlier runs.

## Output/Result

- A provisioned Oracle CNE lab, single or multi-control-plane, with minimal user input.
- Integration with optional features as toggled in variables.
- Oracle CNE config saved for CLI/dev use.

## Example Run

```bash
ansible-playbook deploy_ocne_quick.yml --limit operator
