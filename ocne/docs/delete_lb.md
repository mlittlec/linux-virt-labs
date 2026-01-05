# delete_lb.yml Documentation

## Purpose & Overview

Deletes a private load balancer previously created in Oracle Cloud Infrastructure (OCI) by this automation suite. Ensures cloud resources are freed and incurs no further charges.

## When to Use

Run this playbook when:

- You wish to clean up or decommission lab, test, or demo infrastructure.
- As part of a full cluster/application teardown.

## Key Variables & Inputs

- `private_load_balancer_id`: The unique ID (OCID) of the load balancer to delete. Typically provided as a fact/var from create_lb.yml outputs.

## Task Breakdown

- **Load Balancer Deletion:** Removes the targeted load balancer using `oci_loadbalancer_load_balancer` with state 'absent'.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: load balancer resources.
- Generally called automatically from cleanup logic in create_instance.yml, or can be run stand-alone.
- Needs fact/variable population from previous provisioning plays.

## Output/Result

- Targeted load balancer is deleted from OCI.
- Resources and billing related to this load balancer cease.

## Example Run

```bash
ansible-playbook delete_lb.yml \
  -e "private_load_balancer_id=ocid1.loadbalancer.oc1..."
