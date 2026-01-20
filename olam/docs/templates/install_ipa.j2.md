# install_ipa.j2 Documentation

## Purpose & Overview

Jinja2 template for a shell script that installs FreeIPA non-interactively. Passes domain/realm and admin passwords as Ansible variables for robust lab server deployment.

## When to Use
Render and run as part of manual FreeIPA server installation within Oracle Linux Automation Manager labs, supporting hands-off clustering and identity workflow onboarding.

## Key Variables & Inputs

- ipaserver_realm, ipadm_password, ipaadmin_password
- Set in playbooks or roles using the template

## Task Breakdown

- Renders a shell script with all required flags for non-interactive ipa-server-install
- Handles all parameters (realm, passwords, input mode) for safe, reusable install

## Dependencies & Relationships

- Called by provision_free_ipa.yml (manual setup branch)
- Used for testing or in scenarios where the FreeIPA Ansible role is not used

## Output/Result

- Installs FreeIPA with provided configuration, creating a lab identity server
