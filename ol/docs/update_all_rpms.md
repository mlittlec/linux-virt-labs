# update_all_rpms.yml Documentation

## Purpose & Overview

Automates updates for all installed Oracle Linux packages, ensuring hosts are up-to-date with the latest kernel and software. Can optionally trigger a controlled reboot if a new kernel is installed.

## When to Use

Use to keep all lab VMs or bare-metal hosts fully patched, compliant, and running the latest required updates. Run after provisioning or before critical tests/upgrades.

## Key Variables & Inputs

- Host groups: `server`, `vbox`
- `proxy_env`: Optional proxy settings for package download.

## Task Breakdown

- Uses `dnf` to update all packages to the latest version with retries for robustness.
- Executes needs-restarting to detect if a reboot is required after updates.
- If required, safely reboots the machine(s).
- debug_enabled variable can be set for verbose output and diagnosis.

## Dependencies & Relationships

- Requires default_vars.yml for host/user/high-level configuration.
- Often referenced as a task or imported playbook in create_instance.yml or similar top-level orchestrations.

## Output/Result

- System is updated to latest OS, security patches, and all software packages; will reboot if kernel or core system has changed.
