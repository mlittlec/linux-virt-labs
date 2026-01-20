# deploy_ocne_libvirt.yml — Deploy OCNE on Local KVM/Libvirt Node

This playbook installs Oracle Cloud Native Environment (Oracle CNE) on a KVM/Libvirt-backed Oracle Linux server.

## Purpose

- Installs necessary virtualization packages for Oracle Linux 8/9 (KVM, libvirt, cockpit, containers).
- (Optionally) Installs Oracle CNE and creates a cluster using distribution-recommended repositories and tools.
- Creates a user and configures SSH, sudo, and firewall to enable local hypervisor control.

## Requirements

- Target hosts: Oracle Linux 8 or 9 "server" group members.
- SSH setup, user with sudo privileges (`username`/`user_default_password`).
- Variables from `default_vars.yml` and playbook scope.
- Optionally, `install_ocne_rpm`, `create_ocne_cluster`, `ocne_cluster_name`.

## Key Tasks

1. **Install virtualization packages:**  
   Installs base packages (KVM, libvirt, virt-install, cockpit, etc.) with retry logic based on OS version.

2. **Enable and start services:**  
   Handles switching between Oracle Linux 8 (monolithic) and 9 (modular) systemd service layouts.

3. **Add user to virtualization groups:**  
   Ensures proper permissions for `libvirt`/`qemu`.

4. **Repository enable/block:**  
   If `install_ocne_rpm` is enabled, configures extra Oracle Linux repositories for Oracle CNE.

5. **Install OCNE & cluster tools:**  
   Installs Oracle CNE/kubectl and (optionally) calls `ocne cluster start` to set up a cluster.

## Relationships

- **Deployed from:** `create_instance.yml` for `ocne_type: libvirt`
- **Shares pattern/logical roles with:** `deploy_ocne_oci.yml` (OCI-based) and `deploy_ocne_none.yml` (noop path)
- **Variables sourced from:** Host/group/fact files, `default_vars.yml`

## References

- [Oracle CNE Documentation](https://docs.oracle.com/en/solutions/deploy-cloud-native-environments/index.html)
- [Libvirt Documentation](https://libvirt.org/)
- [oracle.oci.oci_compute_instance](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/)

## Example Usage

<!-- ```yaml
ansible-playbook -i inventory deploy_ocne_libvirt.yml
``` -->

This playbook is typically executed as part of the full workflow via `create_instance.yml`.

