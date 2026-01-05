# provision_builder.yml Documentation

## Purpose & Overview

Automates the setup of an Ansible Builder development node for Oracle Linux Automation Manager/AWX environments—prepares Execution Environments, required dependencies, and builds custom container images for automation jobs.

## When to Use

Run when creating or updating container-based execution environments (EEs) for Oracle Linux Automation Manager, especially when customizing requirements, dependencies, or base configs for advanced automation workflows.

## Key Variables & Inputs

- Hosts: devops-node
- Includes default_vars.yml for user and path setup

## Task Breakdown

- Installs and enables Automation Manager repos, disables old versions
- Ensures lingering and service persistence for the user account
- Installs ansible-builder, ansible-navigator, pip/py
- Creates all required EE project files: execution_environment.yml, ansible.cfg, requirements.yml, requirements.txt, bindep.txt
- Builds EE container using ansible-builder
- Creates a project directory and sample playbook for manual or automated EE testing
- Handles all OS variations, permissions, and enables debug diagnostics

## Dependencies & Relationships

- Consumed by OLAM devops nodes, typically from create_instance.yml or as a post-provision step for advanced automation
- Uses multiple templates for EE construction (execution_environment.yml.j2, requirements.yml.j2, bindep.txt.j2, requirements.txt.j2, playbook.yml.j2)

## Output/Result
