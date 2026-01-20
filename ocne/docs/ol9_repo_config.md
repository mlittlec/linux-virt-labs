# ol9_repo_config.yml Documentation

## Purpose & Overview

Configures Oracle Cloud Native Environment (Oracle CNE) YUM repositories on Oracle Linux 9. Ensures hosts only use up-to-date, compatible package sources for deploying Oracle CNE/Kubernetes and disables legacy or conflicting repos.

## When to Use

Use this playbook:

- When building or updating OL9-based Oracle Linux nodes for Oracle CNE automation.
- Prior to package installation, component update, or if repo configurations have changed.

## Key Variables & Inputs

- `ol9_enable_repo`: YUM repo(s) to enable for OL9 (from default_vars.yml).
- `ol9_disable_repo`: YUM repo(s) to disable for OL9.

## Task Breakdown

- **Install olcne release package:** Adds required Oracle CNE repo stubs for OL9 platform.
- **Enable Current Repos:** Enables modern, required Oracle CNE-specific and base OS repos.
- **Disable Old/Conflicting Repos:** Turns off outdated or experimental sources for reliability and support.

## Dependencies & Relationships

- Must be run as root.
- Sibling to ol8_repo_config.yml (OL8 equivalent).
- Generally called early in OCI cluster and Oracle CNE deployments.

## Output/Result

- The node is fully prepared with all required repositories for Oracle CNE install on OL9, reducing chances of package/test failures due to repo mismatch.

## Example Run

```bash
ansible-playbook ol9_repo_config.yml
