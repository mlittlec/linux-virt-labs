# configure_passwordless_ssh.yml Documentation

## Purpose & Overview

Enables password-less SSH for all engine and KVM hosts, streamlining secure automation by exchanging public keys and ensuring mutually trusted authorized_keys/known_hosts configuration.

## When to Use

Use after host or VM provisioning to prepare the cluster for agentless orchestration, configuration management, or delegation of control from Ansible Tower/AWX/Oracle Linux Automation Manager.

## Key Variables & Inputs

- Hosts: engine, kvm (excluding localhost)
- `username`: User account on target systems.
- `private_key`: Basename for SSH private/public key files.
- Uses all hosts in inventory minus localhost for key distribution.

## Task Breakdown

- For each host: generates an SSH keypair for `username` user (if missing).
- Fetches public keys from all hosts and distributes them via authorized_keys.
- SSH known_hosts is populated for peer recognition and trust.
- Optionally prints hostvars and network info when debugging is enabled.

## Dependencies & Relationships

- Requires SSH reachability between all hosts.
- Referenced by main orchestrator and initial lab bring-up playbooks.

## Output/Result
