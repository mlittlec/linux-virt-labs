# execution_environment.yml.j2 Documentation

## Purpose & Overview

Jinja2 template for building custom Execution Environment (EE) container images for AWX/Oracle Linux Automation Manager with Ansible Builder. Specifies all dependencies and base image logic required for repeatable, production-grade container automation.

## When to Use

File is rendered whenever building a new or updated EE container for Oracle Linux Automation Manager labs—ensuring module, pip, and OS/package dependencies are met.

## Key Variables & Inputs

- Distribution major version (OL8/OL9) chooses correct base image.
- Requires requirements.yml, requirements.txt, bindep.txt as dependency files.

## Task Breakdown

- Template logic sets up pip, system, and galaxy dependency lists.
- Selects correct EE image from Oracle Container Registry, prepping for builder pulls.
- Provides custom pre and post build steps for image labeling, shell debugging, and extension.

## Dependencies & Relationships

- Consumed by provision_builder.yml whenever building EEs.
- Works with all supporting dependency files and ansible.cfg.

## Output/Result

- Defines a versioned, dependency-aware EE container image—critical for AWX/Oracle Linux Automation Manager automation and modern CI/CD labs.
