# provision_git_server.yml Documentation

## Purpose & Overview

Fully automates the setup of a basic Git server for lab/CI testing, ensuring the presence of a “git” user, SSH key management, bare repo initialization, and default branch configuration.

## When to Use

Use to enable version control, CI/CD, or repo hosting within a self-contained OLAM lab; ideal for demonstrating GitOps or onboarding new users to Git-centric workflows in labs or sandboxes.

## Key Variables & Inputs

- Hosts: git-server
- `ssh_key_params`, `private_key`, all credential inputs for the git user

## Task Breakdown

- Adds “git” user and authorizes SSH key for secure access
- Installs git with retries for idempotency
- Creates initial bare repository at the fixed path (hello-world.git)
- Sets default branch to “main”
- Initializes/creates repo with git, supporting push/pull semantics for lab team

## Dependencies & Relationships

- Assumes git-server host group, must be present and accessible
- SSH key must be provisioned and present for secure auth by clients

## Output/Result
