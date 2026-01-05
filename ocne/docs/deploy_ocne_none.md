# deploy_ocne_none.yml Documentation

## Purpose & Overview

This playbook serves as a "no operation" (noop) placeholder for environments or situations where Oracle CNE does not need to be installed. It is referenced conditionally by orchestration playbooks to satisfy logic branches without side effects.

## When to Use

- When running test, demonstration, or controlled pipelines where Oracle CNE deployment should be skipped.
- As a logic target for inventory or playbook inclusion workflows that require a no-op phase.

## Key Variables & Inputs

- None required.

## Task Breakdown

- Prints an informational message confirming that no Oracle CNE deployment will occur.

## Dependencies & Relationships

- Invoked by create_instance.yml and main orchestration logic conditional on the `ocne_type` variable.
- Makes no changes or system modifications.

## Output/Result

- A clear debug/info message; no cloud or local resources changed.

## Example Run

```bash
ansible-playbook deploy_ocne_none.yml --limit operator
