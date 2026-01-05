# ovirt_connection_test.yml Documentation

## Purpose & Overview

Tests connection to the oVirt/Oracle Linux Virtualization Manager engine, ensuring API/service reachability and basic authentication before continuing cluster configuration or maintenance.

## When to Use

Run just after engine and host provisioning, before triggers like host/network/add/import or D/R orchestration.

## Key Variables & Inputs

- Engine address and credentials (host/group vars, default_vars.yml)

## Task Breakdown

- Establishes a connection to the oVirt/Oracle Linux Virtualization Manager API endpoint.
- Validates credentials, outputs status or error.

## Dependencies & Relationships

- Precedes any cluster management tasks that require engine communication.
- Reports issues before downstream failures occur.

## Output/Result
