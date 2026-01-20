# host_setup.yml Documentation

## Purpose & Overview

Prepares remote hosts for lifecycle management: ensures user account, sudo access, SSH configuration, root filesystem growth, and secure locale/permissions—all with robust connection and retry handling.

## When to Use

Include this playbook at the start of any host preparation flow (after initial provisioning and networking). Ensures Ansible and default users are correctly configured for further configuration or automation.

## Key Variables & Inputs

- `username`, `user_default_password`, `private_key`: User, password, and SSH key info.
- `max_retries`, `retry_delay`: Controls connection resilience and timed retries.
- `debug_enabled`: Toggles status/debug output.

## Task Breakdown

- Waits for connection or SSH reachability (handles proxy/non-proxy).
- Optionally grows root filesystem post-provisioning.
- Creates user, sets password, sudoers (NOPASSWD), and authorized SSH key.
- Configures locale/export in .bashrc.
- Creates .ansible tmp directory for temporary files/operations.
- Optionally logs denied packets via firewalld for troubleshooting.
- Implements retry and rescue mechanism—if setup fails, will wait and re-try processing.
- All commands are idempotent and allow for failed or interrupted provisioning runs to safely recover.

## Dependencies & Relationships

- Consumed by: create_instance.yml, initial provisioning flows, or anywhere user/account/config bootstrapping is required.

## Output/Result

- All remote hosts are fully configured for subsequent roles, automated workflows, and secure SSH-driven automation.
