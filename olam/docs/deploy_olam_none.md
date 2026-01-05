# deploy_olam_none.yml Documentation

## Purpose & Overview

A “no operation” (noop) playbook for Oracle Linux Automation Manager. Used to satisfy workflow requirements when cluster deployment or setup is intentionally skipped.

## When to Use

Serve as a placeholder for tasks, demo, or CI flows that want to disable Oracle Linux Automation Manager deploy logic without breaking dependency chains.

## Key Variables & Inputs

- None; runs on hosts in the control group.

## Task Breakdown

- Prints a message to the log stating that this is a noop and Oracle Linux Automation Manager is not being deployed.

## Dependencies & Relationships

- Used by create_instance.yml and as an import/inclusion target when skipping Oracle Linux Automation Manager deploy or install logic in conditionals.

## Output/Result

- No changes made; simply logs intention for auditing/clarity in automation output.
