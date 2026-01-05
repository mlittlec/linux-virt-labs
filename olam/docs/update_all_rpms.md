# update_all_rpms.yml Documentation

## Purpose & Overview

Automates system-wide package updates for all Oracle Linux Automation Manager lab VMs or physical hosts, testing for required kernel reboots and performing them safely.

## When to Use

Run after any automated deployment, prior to integration testing, or as part of a regular maintenance schedule to apply OL security, feature, or bugfix updates.

## Key Variables & Inputs

- Applies to all:!localhost:!remote by default.
- `debug_enabled` can be set for output control.

## Task Breakdown

- Updates all O/S packages to latest with dnf, handling network/proxy as necessary.
- Ensures a safe, reliable upgrade with retries for open network scenarios.
- Detects if a reboot is required via needs-restarting and reboots only when necessary.
- Prints reboot requirement for admin attention on demand.

## Dependencies & Relationships

- Relies on host setup/credential priming first.
- Should be run as an include/import by create_instance.yml, lab reset scripts, or CI/CD pipelines.

## Output/Result
