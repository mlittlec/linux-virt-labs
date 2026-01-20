# receptor.conf.j2 Documentation

## Purpose & Overview

Single-node receptor configuration for local/hybrid automation in Oracle Linux Automation Manager labs. Sets node ID, debug log level, TCP listener, control services, and selects local or ansible-runner worktype for command processing.

## When to Use

Use when running Oracle Linux Automation Manager/AWX labs or demos on individual lab nodes, for single-host or illustrative “mini-cluster” automation.

## Key Variables & Inputs

- control_node_ip (unique node ID)
- group_names controls worktype (local for control/hop, ansible-runner otherwise)

## Task Breakdown

- Configures basic receptor node settings
- Enables hybrid “local/ansible-runner” worktype switching for demo or test labs
- No complex logic; renders YAML for direct consumption

## Dependencies & Relationships

- Compared/paired with receptor_cluster.conf.j2 for full topology builds
- Used as a drop-in config for “test bench” labs or onboarding

## Output/Result

- Valid YAML receptor config for single-node/lab automation
