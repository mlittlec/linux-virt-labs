# configure_passwordless_ssh.yml Documentation

## Purpose & Overview

Automatically establishes password-less SSH access between all lab hosts (excluding localhost and "remote") by generating SSH key pairs, exchanging and installing public keys, and pre-populating known_hosts to avoid SSH prompts.

## When to Use

Run anytime after instantiation or when machine/user trust is needed for seamless automation, job dispatch, or inter-node orchestration.

## Key Variables & Inputs

- `username`: User to receive and use generated keys.
- `private_key`: SSH private key basename (default: id_rsa).
- `ansible_play_hosts_all`: Dynamic host list; excludes "localhost".
- `my_subnet_domain_name`: Used for accurate inventory/trust.
- debug_enabled: Prints out hostvars for host state checks.

## Task Breakdown

- Generates SSH keypair for target user.
- Fetches and gathers all host public keys.
- Loops through hosts to update authorized_keys and populate known_hosts.
- Trusted host connectivity for all remaining workflow steps.

## Dependencies & Relationships

- Requires SSH access to all hosts.
- Downstream dependency for automation modules that require agentless, password-less connections (Ansible, scp, rsync, etc).

## Output/Result
