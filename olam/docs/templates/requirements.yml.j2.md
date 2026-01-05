# requirements.yml.j2 Documentation

## Purpose & Overview

Template for an Ansible requirements.yml for Galaxy collection dependencies—used by builder pipelines to ensure all Oracle OCI and oVirt modules are present in Execution Environments or CI/CD lab orchestration.

## When to Use

Include as requirements.yml in container and EI builds for automated dependency installation before executing test or onboarding tasks in Oracle Linux Automation Manager labs.

## Key Variables & Inputs

- Pinning of oracle.oci (>=5.5.0,<6.0.0) and inclusion of ovirt.ovirt

## Task Breakdown

- Outputs the Galaxy collection dependency list for inclusion by ansible-galaxy or ansible-builder

## Dependencies & Relationships

- Paired with requirements.txt, bindep.txt, and execution_environment.yml.j2 for a complete dependency lifecycle.
- Referenced by provision_builder.yml during container/EE build and lab orchestration flows.

## Output/Result

- All required modules and roles available for repeatable, reliable OLAM automation.
