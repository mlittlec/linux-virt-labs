# playbook.yml.j2 Documentation

## Purpose & Overview

Template for a sample Ansible playbook to retrieve the OCI object storage namespace, validating Execution Environment setup and OCI module integration in Oracle Linux Automation Manager labs.

## When to Use

Use as a test or demo playbook to confirm OCI automation capabilities and correct connectivity within EEs or builder flows.

## Key Variables & Inputs

- No user input; runs on localhost.
- Calls oracle.oci.* module to access current tenancy namespace.

## Task Breakdown

- Runs a single task: fetches OCI object storage namespace and registers it in the output var.
- Prints the returned result as a debug message.

## Dependencies & Relationships

- Used by builder testing, CI workflows, or onboarding/EE validation.

## Output/Result

- Debug output containing the user's tenancy namespace—proof of successful OCI module operation and EE config.
