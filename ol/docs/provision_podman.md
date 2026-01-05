# provision_podman.yml Documentation

## Purpose & Overview

Installs Podman (and related container tools, including `podman-docker` and `conntrack`) and curl for running Linux containers. Handles differences between Oracle Linux 8/9/10.

## When to Use

Use to ready a server for modern container workflows, OCI image deployment, and OCI/Podman automation. Can be included as a child playbook of a larger provisioning/CI process.

## Key Variables & Inputs

- Host group: `server`
- OS version detection: differentiates install approach for OL8, OL9, OL10

## Task Breakdown

- Installs the right set of container tools and dependencies per Oracle Linux version using dnf (tries/retries for robustness).
- Podman and podman-docker for container management (OL9/10), container-tools meta package for OL8.
- Installs `conntrack` and curl everywhere (dependency for K8s/minikube/network testing).

## Dependencies & Relationships

- Requires root/become.
- Used by: create_instance.yml or as a standalone utility playbook.
- May be invoked alongside other VM, VNC, KVM, or infra provisioning.

## Output/Result
