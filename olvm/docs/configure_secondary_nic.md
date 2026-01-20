# configure_secondary_nic.yml Documentation

## Purpose & Overview

Configures a secondary network interface on engine and kvm nodes in Oracle Linux Virtualization Manager automation labs. Ensures static IP, DNS, and cluster communication readiness across lab nodes.

## When to Use

Run after instance provisioning when a secondary NIC is required for out-of-band management, storage traffic, or advanced networking scenarios.

## Key Variables & Inputs

- Hosts: engine, kvm
- Gathers all core instance, Virtual Network Interface Card (VNIC), and subnet information via facts modules and OCI API.

## Task Breakdown

- Finds instance/vnic IDs and details for the second NIC (via OCI modules and json_query).
- Retrieves the secondary VNIC’s private IP and MAC address.
- Sets up the correct local system network interface, assigning static IP/DNS.
- Checks if the host is already part of an Oracle Linux Virtualization Manager cluster before making changes (for idempotency).
- Adds DNS and network IP config atomically and securely.

## Dependencies & Relationships

- Requires prior setup of default_vars.yml and oci_vars.yml.
- Invoked by create_instance.yml or cluster-setup orchestrators.

## Output/Result
