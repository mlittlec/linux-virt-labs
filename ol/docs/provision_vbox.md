# provision_vbox.yml Documentation

## Purpose & Overview

Automates installation and configuration of Oracle VirtualBox, its extension pack, and supporting dependencies on an Oracle Linux lab host for VM creation and management.

## When to Use

Use to bootstrap a new VirtualBox hypervisor for lab/test/kitchen environments on OL8 systems (primarily intended for desktop or test-lab use cases).

## Key Variables & Inputs

- `virtualbox_version`, `virtualbox_extpack_version`: Determines which version (and extension pack) will be installed.
- `username`: Controls permissions/ownership.
- EPEL, RPM keys, repo configuration: for package management and VirtualBox repo.
- Defaults and proxy variables handled in upstream config.

## Task Breakdown

- Installs EPEL and required dependencies (kernel headers, gcc, make, etc).
- Adds RPM keys, configures the official VirtualBox repo.
- Installs the specified VirtualBox version via dnf/yum.
- Checks for existing extension packs; downloads and installs if missing.
- Handles known package/repo/re-keying order for idempotent deploys.

## Dependencies & Relationships

- Used by: create_instance.yml or as a dedicated VirtualBox lab setup playbook.
- Links with base images and downstream VM create scripts for full virtualized testbeds.

## Output/Result
