# create_instance.yml Documentation

## Purpose & Overview

Top-level playbook for full-lifecycle lab provisioning and teardown—including compute, networking, security rules, and Oracle Linux Automation Manager stack (installer, builder, PAH, FreeIPA, Git, etc.).

## When to Use

Use to launch environments for development, QA, or training with Oracle Linux Automation Manager, Ansible AWX, and supporting tools in a self-contained, repeatable way.

## Key Variables & Inputs

- See default_vars.yml for: compute_instances, base images, SSH/user, toggles for OLAM/PAH/FreeIPA/Git/KVM/VNC/etc.
- OCI config and cloud variables from facts and included var files.
- State file (.ansible-state) used for tracking resource OCIDs.

## Task Breakdown

- Gathers facts and creates all OCI infrastructure: VCN, subnet, gateways, route tables, security lists, instances.
- Builds hosts and configures inventory, SSH, and facts (build.yml, block.yml).
- Sets up base users and agentless automation via host configuration playbooks.
- Delegates installer/finalization flows for Oracle Linux Automation Manager cluster, builder, PAH, FreeIPA, Git, KVM, VNC based on feature flags.
- Ensures end-to-end readiness of all core and optional services.
- Teardown handled via terminate_instance.yml, removes all artifacts.

## Dependencies & Relationships

- Central orchestrator: calls/integrates every major automation playbook/template in olam/.
- Requires all key variables and inventory correct in advance.

## Output/Result
