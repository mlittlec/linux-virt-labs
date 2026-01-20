# create_instance.yml Documentation

## Purpose & Overview

Orchestrates the end-to-end setup, configuration, and teardown of a complete Oracle Linux and Kubernetes test environment on Oracle Cloud Infrastructure (OCI). This master playbook provisions all key resources: network (VCN, subnets, security lists), compute instances (operator, control plane, worker nodes), file storage, load balancer, and OCI registry. It supports lifecycle operations from initial build to cleanup.

## When to Use

Use this playbook to stand up or tear down a full stack for cloud-native/Oracle CNE testing or training on OCI, especially when you need an automated, parameterized, and fully documented workflow covering all infra as code.

## Key Variables & Inputs

- Major variables include (but are not limited to):
  - `ad_placement`: Placement index for OCI Availability Domains
  - `compartment_id`, `compartment_name`: The target OCI compartment(s)
  - `os`, `os_version`, `instance_shape`, `instance_ocpus`, `instance_memory`: Image and sizing details
  - `private_key`: SSH private key
  - `use_vlan`, `use_fss`, `use_lb`, `use_ocir`: Feature toggles
  - Inventory and cluster node definition via `compute_instances`
  - Many variables sourced from imported files: `default_vars.yml`, `oci_vars.yml`, `lb_vars.yml`, etc.
  - Extra-vars can be supplied at runtime to override defaults

## Task Breakdown

1. **Network Setup:**  
   - Creates VCN, subnets, IGW, route tables, security lists, and configures rules (via Jinja2-ingressed templates).
2. **Image Lookup:**  
   - Locates and caches latest Oracle Linux image matching parameters.
3. **Instance Provisioning:**  
   - Builds operator, control, and worker nodes via included build.yml, using looped variables for flexible topologies.
4. **Storage & Load Balancing (Conditional):**  
   - Invokes playbooks for FSS (create_fss.yml), LB (create_lb.yml), and OCIR (create_ocir.yml) when enabled.
5. **Cluster Installation:**  
   - Runs host config (host_setup.yml), IP/VLAN assignment, and then launches main platform install (via deploy_ocne_*.yml).
6. **Reporting:**  
   - Shows host inventory, addresses, and registry information for later use.
7. **Teardown:**  
   - Optionally runs deletion playbooks for all resources, including cloud infra and local artifacts.

## Dependencies & Relationships

- **Imports/Includes:**  
  - Includes and relies on logic/outputs from: build.yml, create_fss.yml, create_lb.yml, create_ocir.yml, delete_fss.yml, delete_lb.yml, delete_ocir.yml, deploy_ocne_*.yml, and Jinja2 templates for security rules/vars.
- **Module Requirements:**  
  - OCI Ansible Collection (compute, network, load balancer, file storage, identity modules, etc.)
- **Artifacts:**  
  - Writes config and generated vars files (oci_vars.yml, lb_vars.yml, fss_vars.yml)

## Output/Result

- A complete, ready-to-use OCI test or demo environment
- Inventory of public/private IPs, registry and storage info
- Ability to destroy and clean up all cloud resources in a controlled way

## Example Run

```bash
ansible-playbook create_instance.yml \
  -e "compartment_id=..." \
  -e "use_vlan=true" \
  -e "use_fss=true" \
  -e "use_lb=true" \
  -e "use_ocir=true"
```

## Teardown Example

```bash
ansible-playbook create_instance.yml --tags "teardown" \
  -e "compartment_id=..."
