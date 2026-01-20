# provision_free_ipa.yml Documentation

## Purpose & Overview

Deploys and configures a FreeIPA server for centralized authentication and identity management as part of an Oracle Linux Automation Manager lab or testbed.

## When to Use

Run to enable Kerberos-based SSO, directory, or DNS support for instances in testing/training environments.

## Key Variables & Inputs

- Hosts: ipa-server
- ipaserver_domain, ipaserver_realm, ipaadmin_password, ipadm_password

## Task Breakdown

- Calls freeipa.ansible_freeipa.ipaserver Ansible role for full FreeIPA install (recommended path for Oracle Linux Automation Manager/Ansible lab setups)
- Variables set to align with facts and lab domain environment.

## Dependencies & Relationships

- Requires the freeipa.ansible_freeipa Galaxy role/collection.
- Integrated into overall lab via inventory/host groups ("ipa-server").
- Downstream effect for host setup, authentication, and seamless SSO.

## Output/Result
