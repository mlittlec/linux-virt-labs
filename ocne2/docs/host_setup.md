# host_setup.yml — Configure Instance OS for OCI & Kubernetes

This playbook prepares newly-provisioned OCI compute instances for secure automation, SSH access, and Kubernetes/operator readiness.

## Purpose

- Automates core Linux setup and hardening for Ansible and operations team management.
- Ensures all necessary OS-level configuration and user access for cluster or application workloads.

## Main Tasks

1. **Handle connectivity/retry logic:**  
   Waits for SSH access and performs retry logic to handle slow cloud bootstraps.

2. **Setup facts/locale:**  
   Collects system info, grows root filesystem, and sets up shell locale.

3. **User management:**  
   Creates a privileged user (`username`), adds SSH keys, and enables password-less sudo.

4. **SSH key management:**  
   Generates new SSH keypair, distributes authorized keys for password-less intra-cluster access.

5. **Inventory/host information:**  
   Prints variables and host inventory when debugging is enabled.

6. **Firewall/basics:**  
   Opens firewall, logs denied packets (if debugging), accepts/records new SSH fingerprints for all hosts.

7. **Rescue/retry:**  
   If a setup step fails, the playbook retries up to `max_retries` times before aborting.

## Requirements

- Linux instance booted on OCI, accessible via generated private key.
- Variables: `username`, `user_default_password`, others from `default_vars.yml`.
- Ansible/SSH access from the orchestrating host.

## Relationships

- **Run by:** `create_instance.yml` for all active inventory hosts.
- **Populates:** SSH setup and Ansible workspace for downstream provisioning.

## References

- [Ansible user module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html)
- [Ansible authorized_key module](https://docs.ansible.com/ansible/latest/collections/ansible/posix/authorized_key_module.html)

## Example Usage

Called for each deployed VM in the main orchestration:

```yaml
- name: Configure instance
  include_tasks: "host_setup.yml"

