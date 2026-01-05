# Oracle Linux Virtualization Manager Automation Lab – Playbooks & Template Documentation

## Overview

This directory provides detailed documentation for every Ansible playbook and Jinja2 template found in `linux-virt-labs/olvm/`.

- Each lab file is mapped 1:1 to a Markdown (`.md`) doc for rapid navigation, debugging, and advanced scenario-building.
- Use this index to onboard, extend, or troubleshoot end-to-end Oracle Linux Virtualization Manager cluster/lab automation.

---

## Deploy Oracle Linux Virtualization Manager Using These Ansible Playbooks

**Note:** If running in your own tenancy, read the `linux-virt-labs` GitHub project [README.md](https://github.com/oracle-devrel/linux-virt-labs) and complete the prerequisites before deploying the lab environment.

1. Open a terminal on the Luna Desktop.

1. Clone the `linux-virt-labs` GitHub project.

   ```shell
   git clone https://github.com/oracle-devrel/linux-virt-labs.git
   ```

1. Change into the working directory.

   ```shell
   cd linux-virt-labs/olvm
   ```

1. Install the required collections.

   ```shell
   ansible-galaxy collection install -r requirements.yml
   ```

1. Update the Oracle Linux Instance Configuration.

   ```shell
   cat << EOF | tee instances.yml > /dev/null
   compute_instances:
   1:
      instance_name: "olvm"
      type: "engine"
      instance_ocpus: 2
      instance_memory: 32
   2:
      instance_name: "olkvm01"
      type: "kvm"
      instance_ocpus: 8
      instance_memory: 64
   3:
      instance_name: "olkvm02"
      type: "kvm"
      instance_ocpus: 8
      instance_memory: 64
   use_vnc_on_engine: true
   EOF
   ```

1. Create an Inventory file.

   ```shell
   cat << EOF | tee hosts > /dev/null
   localhost ansible_connection=local ansible_connection=local ansible_python_interpreter=/usr/bin/python3.6
   EOF
   ```

1. Deploy the lab environment.

   ```shell
   ansible-playbook create_instance.yml -i hosts -e "@instances.yml"
   ```

   **Note:** The free lab environment requires the extra variable `localhost_python_interpreter`, which sets `ansible_python_interpreter` for plays running on localhost. This variable is needed because the environment installs the RPM package for the Oracle Cloud Infrastructure SDK for Python, located under the python3.6 modules. Either remove, or update this variable to match your system's requirements.

   The default deployment shape uses the AMD CPU and Oracle Linux 8. To use an Intel CPU or Oracle Linux 9, add -e instance_shape="VM.Standard3.Flex" or -e os_version="9" to the deployment command.

   > **Important:** Wait for the playbook to run successfully and reach the pause task. At this stage of the playbook, the installation of Oracle Cloud Native Environment is complete, and the instances are ready. Take note of the previous play, which prints the public and private IP addresses of the nodes it deploys and any other deployment information needed while running the lab.

---

## Documentation Table

### Playbooks – Index

| Playbook | Summary | Related Templates/Files |
| --- | --- | --- |
| [build.yml](./build.md) | Provisions OCI compute instances, VNICs, VLAN, block storage. | create_block_storage.yml |
| [check_instance_available.yml](./check_instance_available.md) | Connectivity/fact check for all engine/KVM hosts. | |
| [configure_passwordless_ssh.yml](./configure_passwordless_ssh.md) | Sets up SSH trust between all engine/KVM hosts. | |
| [configure_secondary_nic.yml](./configure_secondary_nic.md) | Configures secondary NICs on engine/KVM hosts for cluster/storage. | etc_hosts_kvm.j2 |
| [create_block_storage.yml](./create_block_storage.md) | Attaches OCI block storage to VMs for storage domains. | |
| [create_hostfile_secondary_nic.yml](./create_hostfile_secondary_nic.md) | Writes secondary NIC hosts aliasing for KVM/engine. | etc_hosts_kvm.j2 |
| [create_instance.yml](./create_instance.md) | Top-level cluster orchestrator—bootstraps infra, nodes, overlay net, etc. | Most plays/templates |
| [create_vlan.yml](./create_vlan.md) | Provisions VLAN & NSG for advanced networking. | |
| [default_vars.yml](./default_vars.md) | Variables for all KVM, engine, storage, and cluster config. | |
| [ovirt_add_hosts.yml](./ovirt_add_hosts.md) | Adds KVM hosts to the Oracle Linux Virtualization Manager/oVirt engine/cluster. | |
| [ovirt_add_logical_network.yml](./ovirt_add_logical_network.md) | Adds logical network to Oracle Linux Virtualization Manager cluster for segmentation/testing. | |
| [ovirt_add_storage_domain.yml](./ovirt_add_storage_domain.md) | Adds storage domains for VMs and ISO images. | |
| [ovirt_check_hosts.yml](./ovirt_check_hosts.md) | Verifies KVM/oVirt hosts are online, ready, and registered. | |
| [ovirt_connection_test.yml](./ovirt_connection_test.md) | Tests connection/auth to Oracle Linux Virtualization Manager engine before further steps. | |
| [ovirt_create_vm_from_template.yml](./ovirt_create_vm_from_template.md) | Creates VMs from pre-imported templates. | |
| [ovirt_delete_host.yml](./ovirt_delete_host.md) | Removes KVM hosts from the cluster and decommissions. | |
| [ovirt_import_template.yml](./ovirt_import_template.md) | Imports VM/OVA templates to cluster storage domains. | |
| [ovirt_list_hosts.yml](./ovirt_list_hosts.md) | Lists all available KVM/oVirt hosts in the cluster. | |
| [provision_instance_basics.yml](./provision_instance_basics.md) | Core user/SSH/locale prep for all hosts. | |
| [provision_olvm_engine_publickey.yml](./provision_olvm_engine_publickey.md) | Copies engine pubkey to KVM hosts for trust. | |
| [provision_olvm_engine.yml](./provision_olvm_engine.md) | Configures Oracle Linux Virtualization Manager engine & exposes host/cluster for control plane. | answer_file.j2 |
| [provision_vnc.yml](./provision_vnc.md) | Adds VNC/GUI capability to engine nodes in the lab. | |
| [requirements.yml](./requirements.md) | Ansible Galaxy collection dependencies for cluster automation. | |
| [terminate_instance.yml](./terminate_instance.md) | Fully tears down OCI and resets all resource state. | |

---

### Templates – Index

| Template | Description & Usage |
| --- | --- |
| [answer_file.j2](./templates/answer_file.j2.md) | Automated installer answers for unattended setups. |
| [egress_security_rules.j2](./templates/egress_security_rules.j2.md) | YAML variable: allows all TCP traffic from VMs. |
| [etc_hosts_kvm.j2](./templates/etc_hosts_kvm.j2.md) | Generates /etc/hosts entries for secondary-NIC HA labs. |
| [ingress_security_rules.j2](./templates/ingress_security_rules.j2.md) | YAML variable: allows feature-based ports per scenario. |
| [oci_vars.j2](./templates/oci_vars.j2.md) | Core OCI resource mapping file for automated flows. |

---

### Tree Listing

```markdown
└── olvm
    ├── build.md
    ├── check_instance_available.md
    ├── configure_passwordless_ssh.md
    ├── configure_secondary_nic.md
    ├── create_block_storage.md
    ├── create_hostfile_secondary_nic.md
    ├── create_vlan.md
    ├── default_vars.md
    ├── ovirt_add_hosts.md
    ├── ovirt_add_logical_network.md
    ├── ovirt_add_storage_domain.md
    ├── ovirt_check_hosts.md
    ├── ovirt_connection_test.md
    ├── ovirt_create_vm_from_template.md
    ├── ovirt_delete_host.md
    ├── ovirt_import_template.md
    ├── ovirt_list_hosts.md
    ├── provision_instance_basics.md
    ├── provision_olvm_engine_publickey.md
    ├── provision_olvm_engine.md
    ├── provision_vnc.md
    ├── README.md
    ├── requirements.md
    ├── terminate_instance.md
    └── templates
        ├── answer_file.j2.md
        ├── egress_security_rules.j2.md
        ├── etc_hosts_kvm.j2.md
        ├── ingress_security_rules.j2.md
        └── oci_vars.j2.md
```

---

## Relationships and Workflow

- **create_instance.yml** is the primary orchestrator—handles resource setup, network config, host prepping, cluster linking, VNC enable, storage-provisioning, and tear-down.
- **build.yml** provisions core nodes and ensures VMs are cluster-ready.
- **ovirt_add_hosts.yml**, **ovirt_add_logical_network.yml**, **ovirt_add_storage_domain.yml**: incremental cluster topology enablement, networking, and storage.
- **Templates** are always rendered by parent playbooks to automate config, security, or hosts file population.
- Onboarding, troubleshooting, and advanced test scenarios are enabled by per-file documentation and this README.

---

## Tips and Best Practices

- Review each individual Markdown doc for file-specific details, usage notes, and in-context examples.
- Always use version-pinned dependency management via `requirements.yml`.
- Keep code and docs in sync: update the corresponding `.md` file for any playbook, role, or template change.

---

## References

- [Oracle Cloud Infrastructure Documentation](https://docs.oracle.com/en-us/iaas/Content/home.htm)
- [OCI Ansible Collection](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/)
- [Oracle Linux Virtualization Manager](https://docs.oracle.com/en/virtualization/oracle-linux-virtualization-manager/)

---

For troubleshooting, edge use cases, or advanced onboarding, see individual `.md` files in this directory.
