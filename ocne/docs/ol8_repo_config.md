# ol8_repo_config.yml Documentation

## Purpose & Overview

Configures all necessary Oracle Oracle Cloud Native Environment (Oracle CNE) YUM repositories for Oracle Linux 8 hosts. Ensures that only required/compatible repositories are enabled, and older or conflicting ones are disabled, prior to package installation.

## When to Use

Use this playbook:

- When provisioning or resetting any Oracle Linux 8-based VM/instance for Oracle CNE, Kubernetes, or support package installation.
- As a prerequisite step for installing Oracle CNE components or updating the platform on OL8.

## Key Variables & Inputs

- `ol8_enable_repo`: Names of current repositories to enable (from default_vars.yml).
- `ol8_disable_repo`: Legacy or obsolete repositories to disable (from default_vars.yml).

## Task Breakdown

- **Install olcne release package:** Ensures the Oracle CNE release repo is present.
- **Enable Current Repos:** Sets all primary Oracle CNE/Oracle Linux 8 repositories required for new installs.
- **Disable Old/Conflicting Repos:** Shuts off outdated or conflict-prone YUM repo sources.

## Dependencies & Relationships

- Must be run as root (via become).
- Typically called by higher-level deployment playbooks prior to Oracle CNE install.
- Closely paired with ol9_repo_config.yml for OL9 support.

## Output/Result

- Host is left with the latest/official Oracle CNE and OL8 YUM repositories enabled, safer package updates/installations, and fewer repo conflicts.

## Example Run

```bash
ansible-playbook ol8_repo_config.yml
