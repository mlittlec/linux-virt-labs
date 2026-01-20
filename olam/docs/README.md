# Deploy Oracle Linux Automation Manager (OLAM) – Lab Playbook & Template Documentation

## Overview

This directory contains authoritative, onboarding-focused documentation for every Ansible playbook and Jinja2 template in the Oracle Linux Automation Manager lab (`linux-virt-labs/olam/`).

- Each file is mapped 1:1 to a `.md` doc for easy navigation, troubleshooting, and extension.
- Operators, developers, and learners can find detailed usage, logic, and variable descriptions here.

---

## Deploy Oracle Linux Automation Manager Using These Ansible Playbooks

**Note:** If running in your own tenancy, read the `linux-virt-labs` GitHub project [README.md](https://github.com/oracle-devrel/linux-virt-labs) and complete the prerequisites before deploying the lab environment.

1. Open a terminal on the Luna Desktop.

1. Clone the `linux-virt-labs` GitHub project.

   ```shell
   git clone https://github.com/oracle-devrel/linux-virt-labs.git
   ```

1. Change into the working directory.

   ```shell
   cd linux-virt-labs/olam
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
      instance_name: "olam-node"
      type: "control"
   olam_type: none
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
| [block.yml](./docs/olam-original/block.md) | Attaches OCI block storage volumes to compute nodes. | |
| [build.yml](./build.md) | Launch and inventory OCI instances, configures storage/net. | block.yml |
| [check_instance_available.yml](./check_instance_available.md) | Sanity and reachability check for lab/OCI hosts. | |
| [configure_passwordless_ssh.yml](./configure_passwordless_ssh.md) | Sets up password-less SSH trust between all hosts. | |
| [convert_ansible_inventory.sh](./convert_ansible_inventory.sh.md) | Converts nonstandard Ansible inventories to clustered format | Script |
| [create_instance.yml](./create_instance.md) | Top-level orchestrator: bootstraps all infra, Oracle Linux Automation Manager, teardown. | Most plays/templates |
| [default_vars.yml](./default_vars.md) | All central configuration: VM sizing, features, credentials. | |
| [deploy_olam_cluster.yml](./deploy_olam_cluster.md) | Multi-node Oracle Linux Automation Manager cluster install (control/execution/db nodes). | |
| [deploy_olam_none.yml](./deploy_olam_none.md)| No-op for conditional disables or dry-runs. | |
| [deploy_olam_olae.yml](./deploy_olam_olae.md) | Installs ansible-core for basic OLAE only test/dev setups. | |
| [deploy_olam_single.yml](./deploy_olam_single.md) | Single-control-node Oracle Linux Automation Manager deployment for quickstart/testing. | |
| [deploy_olam_v1.yml](./deploy_olam_v1.md) | Deploys legacy (v1) Oracle Linux Automation Manager for migration/compatibility. | |
| [get_facts.yml](./get_facts.md) | Minimal fact gathering utility, prints localhost info. | |
| [provision_builder.yml](./provision_builder.md) | Builds custom Ansible Builder Execution Environments (EE) | execution_environment.yml.j2, requirements.yml.j2, requirements.txt.j2, bindep.txt.j2, playbook.yml.j2 |
| [provision_free_ipa.yml](./provision_free_ipa.md) | Sets up FreeIPA for SSO/directory integration via role. | install_ipa.j2 |
| [provision_git_server.yml](./provision_git_server.md) | Bootstraps a git server and creates initial repo. | |
| [provision_instance_basics.yml](./provision_instance_basics.md) | Ensures all base users, SSH, tags, and locales are ready. | |
| [provision_kvm.yml](./provision_kvm.md) | Installs KVM/libvirt/cockpit for in-lab virtualization. | |
| [provision_pah.yml](./provision_pah.md) | Sets up Private Automation Hub (PAH) for collection sharing. | hosts.j2 |
| [provision_vnc.yml](./provision_vnc.md) | Installs VNC/GUI on devops-node(s) for remote graphical login. | |
| [requirements.yml](./requirements.md) | Lists all Ansible Galaxy collection requirements. | |
| [terminate_instance.yml](./terminate_instance.md) | Deletes OCI lab VMs/resources and prompts for artifact wipe. | |
| [update_all_rpms.yml](./update_all_rpms.md) | Updates all packages, reboots lab hosts as needed. | |

### Templates – Index

| Template | Description & Usage |
| --- | --- |
| [bindep.txt.j2](./templates/bindep.txt.j2.md) | Build/package dependencies for Execution Environments |
| [egress_security_rules.j2](./templates/egress_security_rules.j2.md) | OCI security rule: enables all TCP egress |
| [execution_environment.yml.j2](./templates/execution_environment.yml.j2.md) | Ansible Builder EE build context/deps |
| [hosts.j2](./templates/hosts.j2.md) | Ansible inventory for PAH/single-host setups |
| [ingress_security_rules.j2](./templates/ingress_security_rules.j2.md) | Dynamic ingress rules for OCI security—feature toggled |
| [install_ipa.j2](./templates/install_ipa.j2.md) | Shell install script for FreeIPA server |
| [nginx.conf.j2](./templates/nginx.conf.j2.md) / [nginx.conf_v1.j2](./templates/nginx.conf_v1.j2.md) | Base Nginx config, feature toggled server blocks |
| [oci_vars.j2](./templates/oci_vars.j2.md) | Core OCID variables for templates and automation |
| [playbook.yml.j2](./templates/playbook.yml.j2.md) | Sample EE task playbook: gets OCI storage namespace |
| [receptor_cluster.conf.j2](./templates/receptor_cluster.conf.j2.md) | Config for multi-node receptor mesh (lab HA/cluster) |
| [receptor.conf.j2](./templates/receptor.conf.j2.md) | Single node receptor config, group logic |
| [requirements.txt.j2](./templates/requirements.txt.j2.md) | Python (pip) requirements for EEs |
| [requirements.yml.j2](./templates/requirements.yml.j2.md) | Galaxy collection requirements for EEs |

---

## Tree Listing

```markdown
└── olam
    ├── block.md                                # Attaches OCI block storage volumes to compute nodes.
    ├── build.md                                # Launch and inventory OCI instances, configures storage/net.
    ├── check_instance_available.md             # Sanity and reachability check for lab/OCI hosts.
    ├── configure_passwordless_ssh.md           # Sets up password-less SSH trust between all hosts.
    ├── convert_ansible_inventory.sh.md         # Converts nonstandard Ansible inventories to clustered format.
    ├── create_instance.md                      # Top-level orchestrator: bootstraps all infra, Oracle Linux Automation Manager, teardown.
    ├── default_vars.md                         # All central configuration: VM sizing, features, credentials.
    ├── deploy_olam_cluster.md                  # Multi-node Oracle Linux Automation Manager cluster install (control/execution/db nodes).
    ├── deploy_olam_none.md                     # No-op for conditional disables or dry-runs.
    ├── deploy_olam_olae.md                     # Installs ansible-core for basic OLAE only test/dev setups.
    ├── deploy_olam_single.md                   # Single-control-node Oracle Linux Automation Manager deployment for quickstart/testing.
    ├── deploy_olam_v1.md                       # Deploys legacy (v1) Oracle Linux Automation Manager for migration/compatibility.
    ├── get_facts.md                            # Minimal fact gathering utility, prints localhost info.
    ├── provision_builder.md                    # Builds custom Ansible Builder Execution Environments (EE).
    ├── provision_free_ipa.md                   # Sets up FreeIPA for SSO/directory integration via role.
    ├── provision_git_server.md                 # Bootstraps a git server and creates initial repo.
    ├── provision_instance_basics.md            # Ensures all base users, SSH, tags, and locales are ready.
    ├── provision_kvm.md                        # Installs KVM/libvirt/cockpit for in-lab virtualization.
    ├── provision_pah.md                        # Sets up Private Automation Hub (PAH) for collection sharing.
    ├── provision_vnc.md                        # Installs VNC/GUI on devops-node(s) for remote graphical login.
    ├── README.md                               # This file.
    ├── requirements.md                         # Lists all Ansible Galaxy collection requirements.
    ├── terminate_instance.md                   # Deletes OCI lab VMs/resources and prompts for artifact wipe.
    ├── update_all_rpms.md                      # Updates all packages, reboots lab hosts as needed.
    └── templates
        ├── bindep.txt.j2.md                    # Build/package dependencies for Execution Environments.
        ├── egress_security_rules.j2.md         # OCI security rule: enables all TCP egress.
        ├── execution_environment.yml.j2.md     # Ansible Builder EE build context/dependencies.
        ├── hosts.j2.md                         # Ansible inventory for PAH/single-host setups.
        ├── ingress_security_rules.j2.md        # Dynamic ingress rules for OCI security—feature toggled.
        ├── install_ipa.j2.md                   # Shell install script for FreeIPA server.
        ├── nginx.conf_v1.j2.md                 # Base Nginx config, feature toggled server blocks.
        ├── nginx.conf.j2.md                    # Base Nginx config, feature toggled server blocks.
        ├── oci_vars.j2.md                      # Core OCID variables for templates and automation.
        ├── playbook.yml.j2.md                  # Sample EE task playbook: gets OCI storage namespace.
        ├── receptor_cluster.conf.j2.md         # Config for multi-node receptor mesh (lab HA/cluster).
        ├── receptor.conf.j2.md                 # Single node receptor config, group logic.
        ├── requirements.txt.j2.md              # Python (pip) requirements for EEs.
        └── requirements.yml.j2.md              # Galaxy collection requirements for EEs.
```

---

## Relationships and Workflow

- **create_instance.yml** is the main entrypoint—manages inventory, infra, VMs, all feature provisioning, and teardown.
- **block.yml** and **build.yml** manage granular host, storage, and resource setup.
- **provision_builder.yml**, **provision_git_server.yml**, **provision_free_ipa.yml**, **provision_pah.yml** bring up the full auxiliary stack for complete enterprise-style labs.
- **configure_passwordless_ssh.yml** and **provision_instance_basics.yml** must run early to ensure all hosts are ready to receive further automation.
- All templates can be mapped to their consuming playbooks above—see individual .md docs for specifics and downstream usage.

---

## Tips and Best Practices

- Review each individual Markdown doc for file-specific details, usage notes, and in-context examples.
- Always use version-pinned dependency management via `requirements.yml`.
- Keep code and docs in sync: update the corresponding `.md` file for any playbook, role, or template change.

---

## References

- [Oracle Cloud Infrastructure Documentation](https://docs.oracle.com/en-us/iaas/Content/home.htm)
- [OCI Ansible Collection](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/)
- [Oracle Linux Automation Manager](https://docs.oracle.com/en/operating-systems/oracle-linux-automation-manager/)

---

For troubleshooting, edge use cases, or advanced onboarding, see individual `.md` files in this directory.
