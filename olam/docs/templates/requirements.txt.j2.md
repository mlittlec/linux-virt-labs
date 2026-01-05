# requirements.txt.j2 Documentation

## Purpose & Overview

Template file listing Python packages for Oracle Linux Automation Manager EE/container builds. Ensures all pip dependencies are consistently provided when building/deploying automation runners or test environments.

## When to Use

Include as requirements.txt in ansible-builder or manual container image builds for Oracle Linux Automation Manager/AWX-based labs.

## Key Variables & Inputs

- Statically emits: requests (>=2.4.2), ovirt-engine-sdk-python (>=4.6.2), oci (>=2.155.0)

## Task Breakdown

- Used in container build context to install all required pip packages

## Dependencies & Relationships

- Paired with requirements.yml, bindep.txt, and execution_environment.yml.j2 for complete dependency management.

## Output/Result

- Ensures that EEs and containers have the correct Python libraries for OCI/oVirt/requests-based automation.
