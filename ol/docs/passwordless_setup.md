# passwordless_setup.yml Documentation

## Purpose & Overview

Automates generation and distribution of SSH keypairs, ensuring password-less SSH across all lab hosts/inventory. Sets up trusted host keys for seamless multi-host automation.

## When to Use

Run this playbook anytime new hosts are added, SSH keys need to be re-generated, or password-less SSH is required for orchestration, batch job flows, or VM automation.

## Key Variables & Inputs

- `username`: The OS user for whom SSH keys are set up.
- `groups['all']`: Inventory list for trust and key distribution.
- `debug_enabled`, `my_subnet_domain_name`: Toggles and additional host info.

## Task Breakdown

- Generates SSH keypair (openssh_keypair module) for the user if missing.
- Fetches the public key from each host into a local buffer.
- Loops through all hosts and populates authorized_keys with public keys, enabling cross-host access.
- Debug: Optionally prints hostvars and VNC subnet for troubleshooting/diagnostic purposes.
- Ensures all hosts have updated known_hosts, pre-populating fingerprints for secure, non-interactive connections.

## Dependencies & Relationships

- Consumed by create_instance.yml and other initial provisioning/join flows.
- Must be run after base user is set up and SSH access is available.

## Output/Result
