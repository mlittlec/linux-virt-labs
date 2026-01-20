# terminate_instance.yml Documentation

## Purpose & Overview

Safely and fully destroys OCI resources associated with a lab deployment (instances, networking, state files), removing all remote artifacts and only performing local file cleanup after explicit user confirmation.

## When to Use

Use when needing to teardown a lab or demo environment, reclaim cloud resources, or ensure a clean state before next run or integration test.

## Key Variables & Inputs

- State is tracked in `.ansible-state`; only removes if present.
- Uses default_vars.yml for config/location and required credentials.

## Task Breakdown

- Loads state file, ensuring resources are tracked and exist.
- Deletes OCI instances, subnets, security lists, route tables, service gateways, internet gateways, and the VCN.
- Prompts the user interactively before any destructive local action (removal of state, hosts file, buffer dir, etc.).
- Robust error checking to avoid "phantom" deletions if state is unknown.

## Dependencies & Relationships

- Should always be called by (or as part of) create_instance.yml's teardown workflow or caller's clean-up logic.
- Needs the same OCI profile and config context as was used to create.

## Output/Result
