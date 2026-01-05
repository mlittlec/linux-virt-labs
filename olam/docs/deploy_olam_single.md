# deploy_olam_single.yml Documentation

## Purpose & Overview

Automates the full deployment of a single-node Oracle Linux Automation Manager (OLAM) instance, including all dependencies, PostgreSQL database, Nginx, certificates, firewall, and enabling all required services.

## When to Use

Use for test/dev setups or demos where a highly available multi-node cluster is unnecessary; enables Oracle Linux Automation Manager on a single host for rapid evaluation, training, or integration.

## Key Variables & Inputs

- Inventory group: control
- Variables for DB host, AWX instance, user/password configuration from default_vars.yml
- All OS, image, and package info dynamically mapped from facts or configuration.

## Task Breakdown

- Installs required EPEL repos, manages package version locks.
- Sets up Python and pip dependencies for Ansible/AWX/DB.
- Bootstraps PostgreSQL for underlying automation platform storage.
- Downloads and installs the right Oracle Linux Automation Manager version/repo, then enables services.
- Prepares Nginx and Receptor configs, issues SSL certificates, and manages firewalld for required ports.
- Configures all necessary systemd service units and admin credentials.

## Dependencies & Relationships

- Used by create_instance.yml/imported in lab resets or testbed creation.
- Downstream items: integrates with all Inventory/host roles, receives facts from variable files, and feeds other test automation.

## Output/Result
