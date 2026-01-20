# default_vars.yml Documentation

## Purpose & Overview

Defines all key variables, toggles, and defaults for instance creation, VNC/KVM/Podman/VirtualBox provisioning, user credentials, and more in the Oracle Linux automation lab stack.

## When to Use

Use as the central variables file for include_vars or vars_files in playbooks to ensure consistent, DRY provisioning for all automation targets and tools.

## Key Variables & Inputs

- **compute_instances:** List of lab VMs to provision, their names, types, and configs.
- **OS/Image Params:** Operating system, version, base image info for KVM/VBox, and associated SHA256 sum.
- **Resources:** RAM, vCPUs, block size/count, VM disks, networks.
- **Feature Toggles:** Use VNC/KVM/Podman/VBox/etc, boot volume strategy, password-less SSH.
- **User/Password Details:** User, group, default pass, ssh key, root pass.
- **Environment/Build:** Proxy config, region settings, update flags, hypervisor details, image/cert details.
- **Others:** Collection of advanced toggles to enable/disable NFS, HAProxy, OCFS2, etc.

## Dependencies & Relationships

- Used by: All major playbooks (build, create_instance, host_setup, etc.)
- Read by Jinja2 templates via vars_files/include_vars.

## Output/Result

- Parameterizes all hosts, automation tasks, and dependent templates for fully automated, reproducible deployments.
