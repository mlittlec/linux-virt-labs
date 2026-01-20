# host_setup.yml Documentation

## Purpose & Overview

Performs baseline OS/user/SSH/firewall setup on each cluster node before advanced playbook orchestration. Handles user creation, password, sudo, SSH keys, local config, trust relationships, and diagnostics for cloud-native automation environments.

## When to Use

Use this playbook:

- Immediately after provisioning a cluster VM or bare-metal node, before running key deployment or orchestration playbooks.
- To guarantee users, keys, SSH trust, and locale/firewall diagnostics are consistent across all target nodes.

## Key Variables & Inputs

- `username`: The Linux user to be created and configured on each host.
- `user_default_password`: Initial password for the new user (hashed at creation).
- `private_key`: Local SSH private key for use in setting authorized_keys.
- `groups['all']`: Node list for populating trust and key distribution.
- `my_subnet_domain_name`: Used to set fully-qualified host addresses in known_hosts.
- Debug variables: `debug_enabled` for optional diagnostics and additional logging.
- Retry tuning: `max_retries`, `retry_delay` (optional override).

## Task Breakdown

1. **Setup Loop and Retry Handling:**  
   - Tasks run in a block with self-rescue loops; will retry up to `max_retries`.
2. **Connection & Readiness:**  
   - Waits for SSH/connectivity, re-fetches facts.
3. **User and Auth Setup:**
   - Creates user and sets password, group, sudoers (password-less), and locale.
   - Generates and fetches SSH keys.
   - Sets up authorized_keys for seamless Ansible operations.
4. **Trust/Key Distribution:**  
   - Copies local key to each host, populates trusted keys to all inventory peers.
   - SSH known_hosts pre-populated using hostnames, subnets, and IPs (minimizes runtime prompts/failures).
5. **Firewall/Debug (optional):**
   - Optionally enables dropped packet logging for investigation.
6. **Rescue Tasks:**  
   - If setup fails, waits and retries up to max_retries, printing diagnostics and messaging.

## Dependencies & Relationships

- Called from create_instance.yml and other cluster bring-up playbooks.
- Prepares nodes for automation, reducing friction in SSH and host key setup for all Ansible logic that follows.

## Output/Result

- Each node is accessible, has password-less SSH/Ansible access, consistent locale, and correct user/group setup.
- SSH trust and keys ready for Ansible and application orchestration.
- Hardened against common setup failures due to robust self-retry.

## Example Run

```bash
ansible-playbook host_setup.yml
