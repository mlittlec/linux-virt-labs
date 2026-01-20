# get_facts.yml Documentation

## Purpose & Overview

Collects minimal Ansible facts about the localhost. Prints basic host/system info for use in automation validation, debugging, or pre-flight checks.

## When to Use

Run when you want to verify ansible’s local environment, test minimal connectivity, or provide a fact base for lab or CI/CD runs.

## Key Variables & Inputs

- Hosts: localhost

## Task Breakdown

- Uses Ansible’s setup module with "min" gather_subset to collect only necessary system attributes.
- Prints both the gathered facts and current date for status tracking or log auditing.

## Dependencies & Relationships

- Standalone/supporting playbook, not dependent on any global configuration files or host groupings.
- Can be called as a utility by other provisioning or state-validation workflows.

## Output/Result
