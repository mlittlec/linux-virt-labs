# bindep.txt.j2 Documentation

## Purpose & Overview

Jinja2 template for Ansible Builder and Oracle Linux Automation Manager lab automation projects—provides required system packages for container image builds (Execution Environments).

## When to Use

Used during container/EE builds; ensures all extension dependencies for ansible modules, pip, or external tools are pre-installed on OL8/OL9 labs.

## Key Variables & Inputs

- Distribution detection (major version) controls whether python3-devel or python3.11-devel is included.

## Task Breakdown

- Conditional Jinja2 templating: adds different python-devel packages for OL8 vs OL9 for max compatibility.
- Lists OSS/C/SSL/XML libraries crucial for many collections/modules.

## Dependencies & Relationships

- Called from builder pipelines, playbooks, or roles that need container/system-level package lists.

## Output/Result

- bindep.txt included in EE or container build ensures smooth, error-free playbook/module installation.
