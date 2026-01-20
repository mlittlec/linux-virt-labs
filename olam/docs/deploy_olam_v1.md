# deploy_olam_v1.yml Documentation

## Purpose & Overview

Installs Oracle Linux Automation Manager (OLAM) version 1.0 for single-node legacy setups, including all platform dependencies, database (PostgreSQL), Nginx, certificates, firewall/services, and basic facts setup.

## When to Use

Use for compatibility, migration, or demo environments where the original Oracle Linux Automation Manager stack (pre-2.x) is required—either for existing asset support or validation.

## Key Variables & Inputs

- Inventory group: control 
- Variables and image URLs from default_vars.yml
- Service toggles, DB/PKI/Nginx setup variables

## Task Breakdown

- EPEL, pip, and version lock setup
- Oracle Linux Automation Manager 1.0 repo enable; disables other Oracle Linux Automation Manager repositories
- Bootstraps and configures local PostgreSQL backend
- AWX admin/user DB, service/host config, and PKI/cert generation
- Installs/starts Oracle Linux Automation Manager, required firewall ports, cluster ID config, and Nginx

## Dependencies & Relationships

- Requires correct host group and OL base image
- Sets up all database, control, and service infrastructure for AWX/Oracle Linux Automation Manager v1

## Output/Result
