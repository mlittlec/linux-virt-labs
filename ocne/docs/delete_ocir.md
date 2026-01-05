# delete_ocir.yml Documentation

## Purpose & Overview

Removes a private OCI Container Registry (OCIR) repository previously provisioned for the cluster or workloads. This ensures that the registry and all images within are deleted, eliminating storage costs and resource remnants.

## When to Use

Run this playbook when:

- Cleaning up cloud resources after a demonstration, workshop, or test.
- Deleting an obsolete or transient OCI Container Registry.

## Key Variables & Inputs

- `ocir_id`: The unique OCID of the container repository to delete, set by create_ocir.yml.

## Task Breakdown

- **Repository Deletion:** Uses `oci_artifacts_container_repository` to delete the identified registry repository.

## Dependencies & Relationships

- Requires Oracle OCI Ansible Collection: artifacts/container modules.
- Intended for use after create_ocir.yml or as part of the teardown phase in create_instance.yml.
- Requires accurate variable passing for repository OCID.

## Output/Result

- Complete deletion of the targeted OCIR, including all contained images.
- No further storage or billing attached to this registry.

## Example Run

```bash
ansible-playbook delete_ocir.yml -e "ocir_id=ocid1.containerrepo.oc1..xxxx"
