# hosts.j2 Documentation

## Purpose & Overview
Template for a minimal Ansible inventory in YAML format for OLAM labs (PAH/Automation Hub). Maps the "hub" host (the PAH) and assigns user credentials.

## When to Use
Render this template when creating inventory for PAH-related plays, automation runs, or as a base inventory for lab/demo development.

## Key Variables & Inputs

- hub: the target node for collection sharing, admin, or lab orchestration (ol-pah, user oracle).

## Task Breakdown

- Outputs a YAML inventory mapping hosts and ansible_user credentials, consumable by all downstream playbooks or roles requiring inventory input.

## Dependencies & Relationships

- Used by provision_pah.yml and lab onboarding modules for repeatable inventory creation.

## Output/Result

- Static inventory for PAH and lab automation, ready for immediate use or extension.
