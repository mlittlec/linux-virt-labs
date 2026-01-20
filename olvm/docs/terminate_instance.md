# terminate_instance.yml Documentation

## Purpose & Overview

Safely destroys Oracle Linux Virtualization Manager lab compute/networking resources (instances, storage, networking), cleans up all artifacts after explicit user confirmation.

## When to Use
Run when you wish to teardown the Oracle Linux Virtualization Manager lab environment and reclaim all OCI assets. Ensures a clean slate for future automation runs, infrastructure shifts, or migration events.

## Key Variables & Inputs

- State tracked with `.ansible-state` file.
- Uses default_vars.yml for context/config guidance.

## Task Breakdown

- Checks for state before performing any deletion (avoids accidental teardown).
- Deletes all OCI resources: instances, subnets, security lists, route tables, gateways, and VCNs.
- Prompts user to confirm all artifact and state file deletion (buffer, hosts, .ansible-state).
- Robust handling ensures no half-completed destruction.

## Dependencies & Relationships

- Should follow any infrastructure creation process, called explicitly or as a CI/CD teardown job.
- Ensures environment is fully reset for repeatable test/dev workflows.

## Output/Result
