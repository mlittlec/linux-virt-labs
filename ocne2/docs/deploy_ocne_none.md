# deploy_ocne_none.yml — No-Operation Oracle CNE Playbook

This playbook provisions an Oracle Linux 8 or 9 environment fully configured and ready for an Oracle Cloud Native Environment (Oracle CNE) 2 deployment. This is used when a pre-configured Oracle CNE 2 cluster installation is not desired or needed, allowing the user to perform a manual install of Oracle CNE 2 of their choice.

## Purpose

- Serves as an explicit no-operation scenario for environments or test runs where Oracle CNE deployment should be skipped.
- Ensures the provisioning workflow is consistent, regardless of whether Oracle CNE is actually being installed.

## Requirements

- None beyond the Ansible runtime; this playbook simply outputs a message at execution.

## Main Task

- **Prints noop message:**  
  Outputs a message stating that no Oracle CNE cluster is deployed and no changes have been made.

## Relationships

- **Selected by:** The main orchestration playbook (`create_instance.yml`) when `ocne_type: none` or equivalent is set.
- **Complements:** `deploy_ocne_libvirt.yml`, `deploy_ocne_oci.yml`

## Example Output

When run, this playbook will produce:

```yaml
TASK [Print noop message]
ok: [localhost] => {
    "msg": "This is a noop and does not deploy OCNE."
}

