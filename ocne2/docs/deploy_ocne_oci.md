# deploy_ocne_oci.yml — Deploy Oracle CNE Cluster on Oracle Cloud Infrastructure

This playbook provisions and configures an Oracle Cloud Native Environment (Oracle CNE) cluster directly on Oracle Cloud Infrastructure (OCI), including object storage and custom configuration.

## Purpose

- Automates virtualization environment setup for both Oracle Linux 8 and 9.
- Installs and configures Oracle CNE, including repositories, CLI, and supporting components.
- Generates OCI and Kubernetes cluster configuration for direct use with OCI resources.
- Manages object storage (OCI buckets) for images, config, and cluster persistence.
- Optionally deploys File Storage Service (FSS) resources if enabled.

## Requirements

- Targets one or more inventory hosts in the "server" group.
- OS: Oracle Linux 8 or 9. Uses root or sudo-privileged `username`.
- Variables set in `default_vars.yml` and (optional) `oci_vars.yml` for custom resource handling.
- `oracle.oci` collection and other dependencies from `requirements.yml`.

## Key Tasks

1. **Install virtualization packages:**  
   Installs KVM/libvirt/cockpit, with logic for both Oracle Linux 8 and 9.

2. **Start and enable services:**  
   Ensures all needed sockets/services are enabled (modular or monolithic layout).

3. **Install Oracle CNE repositories, CLI, and dependencies:**  
   Adds all required YUM repositories, enables Oracle CNE and developer repos, and installs `ocne`, `kubectl`, Python CLI.

4. **Configure OCI environment:**  
   Copies and edits OCI config files, keys (in `~/.oci/`) for automation.

5. **Create object storage bucket:**  
   Provisions an OCI object storage bucket as image and config store for cluster lifecycle.

6. **Generate cluster YAML config:**  
   Assembles a custom configuration for Oracle CNE using current variable values and cloud resource references.

7. **Deploy FSS deployment descriptors:**  
   Conditionally generates persistent volume and pod descriptors for Kubernetes when `use_fss` is set.

8. **Cluster provisioning:**  
   Calls Oracle CNE CLI to launch the cluster using final config.

## Relationships

- **Triggered by:** `create_instance.yml` when `ocne_type: oci`
- **Shares structure with:** `deploy_ocne_libvirt.yml` (local cluster) and `deploy_ocne_none.yml` (noop)
- **Imports:** Supporting roles/tasks, including `fss_deployments.yml` for storage

## References

- [OCI Ansible Collection](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/)
- [Oracle CNE Documentation](https://docs.oracle.com/en/solutions/deploy-cloud-native-environments/index.html)
- [oracle.oci.oci_object_storage_bucket](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/)

## Example Usage

This playbook is executed automatically as part of the Oracle CNE cluster deployment when the correct type is selected in the main playbook (`create_instance.yml`), but may also be run directly for advanced scenarios:

<!-- ```bash
ansible-playbook -i inventory deploy_ocne_oci.yml -->

