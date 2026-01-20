# deploy_olam_cluster.yml Documentation

## Purpose & Overview

Deploys a highly available, multi-host Oracle Linux Automation Manager (OLAM) cluster, building all control/execution/database components, configuring required repos, postgresql DB, firewall rules, Automation Manager, Nginx, receptors, AWX peering, and optional secrets/certs.

## When to Use

Run when building out an HA (control + execution + DB) test, QA, or training environment for advanced testing or simulation of real-world Oracle Linux Automation Manager production setups.

## Key Variables & Inputs

- Host inventory: control, execution, db groups.
- default_vars.yml for user/SSH, images, and feature toggles.
- Proxy, pip/proxy_env for external access.
- User, DB, credential variables for AWX/Oracle Linux Automation Manager.

## Task Breakdown

- Installs and configures package repos and dependencies (EPEL, psycopg2, OpenSSL, pip, etc).
- Handles version-locking and Python requirements per OL.
- Installs and configures PostgreSQL on DB node, creates users, sets config and port.
- Configures Automation Manager on control/execution.
- Creates ansible project directories, inventory, and config files (nginx, receptor, etc.).
- Runs podman image builds as needed.
- Handles lingering, services, and firewalld for container and automation ports.
- Runs the full Oracle Linux Automation Manager/Automation Manager suite and sets up cluster links.

## Dependencies & Relationships

- Consumes default_vars.yml for nearly all setup.
- References/uses inventory, hosts, group_vars, and integrates with all primary Oracle Linux Automation Manager templates.
- Peers AWX nodes, routers, DB, and configures container execution environments.

## Output/Result
