# create_instance.yml — Orchestration for OCI Environment Provisioning

This is the top-level Ansible playbook that orchestrates network, compute, security, storage, and (optionally) Kubernetes cluster deployment on Oracle Cloud Infrastructure (OCI).

## Purpose

- Automates the full lifecycle of OCI environment provisioning and teardown, including VCN, subnets, security lists, compute instances, block storage, and file storage.
- Manages variables, facts, and inventory to ensure correct resource creation and cross-playbook dependencies.
- Integrates generation and inclusion of dynamic config files and security rules using Jinja2 templates.

## Structure

- Multi-section playbook:
  1. **Gather facts and create instances:** (localhost)  
     Gathers config, creates network (VCN, subnet, security), discovers compartment, generates variables, and sets up required resources using templates and includes (ingress/egress rules, oci_vars, block storage).
  2. **Configure new instances:** (hosts: all)  
     Prepares host environment (users, SSH, provisioning, etc.) via `host_setup.yml`.
  3. **Update all packages:**  
     Optionally runs `update_all_rpms.yml` for system updates and reboots.
  4. **Install Oracle Cloud Native Environment:**  
     Deploys Oracle CNE variant using imported playbooks by type (e.g., `deploy_ocne_libvirt.yml`).
  5. **Provision Podman and container tools:**  
     Optionally installs Podman-related tooling via `provision_podman.yml`.
  6. **Print instance details:**  
     Outputs final connection details and enables manual review.
  7. **Teardown:**  
     Removes all provisioned OCI resources if required, using `delete_fss.yml` and OCI module state=absent.

## Requirements

- Oracle OCI Ansible Collection (`oracle.oci`) and all collections in `requirements.yml`
- Detailed configuration in `default_vars.yml` and optionally in `oci_vars.yml`
- SSH keypair available on localhost for instance bootstrapping
- Optional: Configuration for use of FSS, Podman, or advanced OCNE deployment (via variables)

## Key Included/Imported Files

- `default_vars.yml`: Defaults for all core variables
- `block.yml`: Block storage attachment
- `build.yml`: Compute instance deployment logic (looped for each instance)
- `create_fss.yml` and `delete_fss.yml`: FSS provisioning/teardown
- `host_setup.yml`: User, SSH, and system prep per new instance
- `update_all_rpms.yml`: Updates and reboots (optional)
- `deploy_ocne_libvirt.yml`, `deploy_ocne_oci.yml`, `deploy_ocne_none.yml`: Cluster deployment logic, selected by variable
- Jinja2 templates:  
  - `ingress_security_rules.j2`, `egress_security_rules.j2` (network rules)  
  - `oci_vars.j2`, `fss_vars.j2` (variable output)  
  - `fss_pv.j2`, `fss_pvc.j2`, `fss_pod.j2` (K8s storage manifests)

## Major Variables

See `default_vars.yml` for base values. Some important examples:

| Variable | Description |
| --- | --- |
| `compute_instances` | Dictionary of instance configs, for host loop |
| `use_fss` | Toggle for File Storage Service usage |
| `ocne_type` | Determines which Oracle CNE deployment is installed, via these playbooks:`deploy_ocne_libvirt.yml`, `deploy_ocne_oci.yml`, `deploy_ocne_none.yml` |
| `update_all` | If true, all packages are updated/rebooted |
| ... | Many others; see playbook, defaults, and comments |

## Relationships

- **Orchestration root:** Triggers almost all file logic in `/ocne2`
- **Dynamic includes:** Adapts workflow based on feature flags (FSS, Podman, OCNE type)
- **Reads/generated output:** Inventory facts, variable YAML, K8s manifests for storage
- **Handles teardown and cleanup:** Ensures no orphaned cloud resources

## References

- [OCI Ansible Collection Documentation](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/5.5.0/)
- [Ansible User Guide](https://docs.ansible.com/ansible/latest/index.html)

<!-- ## Example Usage

Run the playbook to orchestrate a full environment:

```bash
ansible-playbook -i inventory create_instance.yml
``` -->

To customize, adjust variables in `default_vars.yml` and inventory as appropriate.

