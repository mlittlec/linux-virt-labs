# receptor_cluster.conf.j2 Documentation

## Purpose & Overview

Jinja2 template for a YAML configuration file supporting mesh/clustered receptor setups in OLAM/AWX labs. Assigns unique node ID per host, sets TCP port, logs, and automation work command.

## When to Use

Generate when configuring distributed or HA automation cluster nodes that require peer-to-peer command and control (e.g., hybrid mesh clusters for lab demos).

## Key Variables & Inputs

- node ID is dynamically rendered from hostvars/inventory facts.
- Standard control services and work command as expected for local runner workflows.

## Task Breakdown

- Exposes all required fields for node mesh/agent setup.
- No logic or conditionals beyond node addressing.

## Dependencies & Relationships

- Used in provision_builder.yml or advanced mesh onboarding flows.
- Paired with single-node receptor.conf.j2 for comparison and lab role teaching.

## Output/Result

- Valid receptor mesh config for lab node, ready for mesh, cluster, or peer automation.
