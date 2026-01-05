# Oracle Linux Lab Automation – Playbooks & Template Documentation

## Overview

This directory provides complete documentation for every Ansible playbook and Jinja2 template found in `linux-virt-labs/ol/`. The goal is to support onboarding, troubleshooting, extension, and integration for users of the Oracle Linux Automation Suite across on-premise, VM, and OCI workflows.

You’ll find an individual Markdown (`.md`) doc for each playbook or template, summarizing purpose, key variables, main logic, dependencies, and real-world usage.

---

## Deploy Oracle Linux Using These Ansible Playbooks

**Note:** If running in your own tenancy, read the `linux-virt-labs` GitHub project [README.md](https://github.com/oracle-devrel/linux-virt-labs) and complete the prerequisites before deploying the lab environment.

1. Open a terminal on the Luna Desktop.

1. Clone the `linux-virt-labs` GitHub project.

   ```shell
   git clone https://github.com/oracle-devrel/linux-virt-labs.git
   ```

1. Change into the working directory.

   ```shell
   cd linux-virt-labs/ol
   ```

1. Install the required collections.

   ```shell
   ansible-galaxy collection install -r requirements.yml
   ```

1. **Customize variables**

   Edit `default_vars.yml` to set your:
   - Compartment, region, and other tenancy details
   - Instance/layout configuration
   - Feature flags (block storage, FSS, Podman, Oracle CNE variant, etc.)

1. Deploy the lab environment.

   Use the `create_instance.yml` file to initiate the creation of a preconfigured environment which you can use to run Oracle CNE. The examples below, while not exhaustive, demonstrate how to deploy different environments using the Oracle CNE 2 repository.

   a. Deploy a single Oracle Linux instance configured with the following:

      - An Oracle user account (used during the installation) with sudo access
      - Key-based SSH, also known as password-less SSH, between the hosts

      ```shell
      ansible-playbook create_instance.yml -e localhost_python_interpreter="/usr/bin/python3.6"
      ```

      This deploys a new Oracle Linux instance with a working libvirt environment ready for you to setup and install an Oracle CNE 2 install of your choice.

   b. The default values (see `defaults.yml`) can be overridden by creating a separate file with the revised definition to be created. This example deploys two Oracle Linux 8 instances with:

   - A non-root user account with sudo access
   - Key-based SSH, also known as password-less SSH, between the hosts
   - Access to the Internet

      - i. Increase the Boot volume size, install libvirt, and use Oracle Linux 9.

         ```shell
         cat << EOF | tee instances.yml > /dev/null
         compute_instances:
         1:
            instance_name: "ol-node-01"
            type: "server"
         2:
            instance_name: "ol-node-02"
            type: "server"
         passwordless_ssh: true
         EOF
         ```

      - ii. Deploy the Oracle Linux instances configured using the custom configuration created in `instances.yml`:

         ```shell
         ansible-playbook create_instance.yml -e localhost_python_interpreter="/usr/bin/python3.6" -e "@instances.yml"
         ```

   **Note:** The free lab environment requires the extra variable `localhost_python_interpreter`, which sets `ansible_python_interpreter` for plays running on localhost. This variable is needed because the environment installs the RPM package for the Oracle Cloud Infrastructure SDK for Python, located under the python3.6 modules. Either remove, or update this variable to match your system's requirements.

   The default deployment shape uses the AMD CPU and Oracle Linux 8. To use an Intel CPU or Oracle Linux 9, add -e instance_shape="VM.Standard3.Flex" or -e os_version="9" to the deployment command.

   > **Important:** Wait for the playbook to run successfully and reach the pause task. At this stage of the playbook, the installation of Oracle Cloud Native Environment is complete, and the instances are ready. Take note of the previous play, which prints the public and private IP addresses of the nodes it deploys and any other deployment information needed while running the lab.

---

## Documentation Table

### Playbooks – Index

| Playbook | Summary | Related Templates/Files |
| - | - | - |
| [block.yml](./block.md) | Provisions and attaches OCI block storage volumes to instances. | Used by build.yml & create_instance.yml |
| [build.yml](./build.md) | Automates compute instance launch & storage, inventories hosts. | block.yml |
| [create_instance.yml](./create_instance.md) | Top-level orchestrator: provisions network, VMs, SSH, and teardown | All major plays/templates |
| [default_vars.yml](./default_vars.md) | Central variable set for all hosts, provisioning, toggles. | Included everywhere |
| [download_ol_iso.yml](./download_ol_iso.md) | Automates Oracle Linux ISO downloads for local builds. | |
| [host_setup.yml](./host_setup.md) | Prepares hosts: users, SSH, sudo, locale, retry-on-fail. | |
| [passwordless_setup.yml](./passwordless_setup.md) | Ensures password-less SSH between all hosts. | |
| [provision_kvm_vm.yml](./provision_kvm.md) | Provisions/reconfigures KVM VMs by pulling images, defining VMs. | vm_template.j2 |
| [provision_kvm.yml](./provision_kvm.md) | Installs/configures KVM hypervisor, delegates to provision_kvm_vm | |
| [provision_podman.yml](./provision_podman.md) | Installs Podman/containers, OL8/9+ variant support. | |
| [provision_vbox.yml](./provision_vbox.md) | Sets up Oracle VirtualBox, repo, keys, and extpack. | |
| [provision_vnc.yml](./provision_vnc.md) | Enables TigerVNC/graphical login for remote desktop access. | |
| [requirements.yml](./requirements.md) | Tracks required Galaxy collections for roles/modules. | |
| [update_all_rpms.yml](./update_all_rpms.md) | Automates dnf update for all packages, safe kernel reboot. | |

### Templates – Index

| Template | Description & Usage |
| --- | --- |
| [egress_security_rules.j2](./templates/egress_security_rules.j2.md) | Default egress: allow all TCP outbound |
| [ingress_security_rules.j2](./templates/ingress_security_rules.j2.md) | Conditional ingress rules for all services (SSH, Nginx, NFS, etc.) |
| [oci_vars.j2](./templates/oci_vars.j2.md) | Emits OCI resource IDs for automation |
| [vm_template.j2](./templates/vm_template.j2.md) | Parameterized libvirt/KVM XML template for VMs |

---

## Tree Listing

```markdown
└-─ ol
    ├── block.md
    ├── build.md
    ├── create_instance.md
    ├── default_vars.md
    ├── download_ol_iso.md
    ├── host_setup.md
    ├── passwordless_setup.md
    ├── provision_kvm_vm.md
    ├── provision_kvm.md
    ├── provision_podman.md
    ├── provision_vbox.md
    ├── provision_vnc.md
    ├── README.md
    ├── requirements.md
    ├── update_all_rpms.md
    └── templates
        ├── egress_security_rules.j2.md
        ├── ingress_security_rules.j2.md
        ├── oci_vars.j2.md
        └── vm_template.j2.md
```

---

## Relationships and Workflow

- **create_instance.yml** is the top-level orchestrator, importing and orchestrating nearly every play/template.
- **build.yml** handles instance creation; relies on block.yml for customizable storage logic.
- **passwordless_setup.yml** and **host_setup.yml** are called for every new environment/user bootstrapping.
- **provision_kvm.yml** and **provision_kvm_vm.yml** manage hypervisors and local/remote VM lab builds, leveraging vm_template.j2 for flexible infrastructure.
- Security (egress/ingress) is always set by including or rendering the Jinja2 templates, enabling full firewall and network policy automation.
- Requirements.yml is to be processed before any playbook run for reliable workflow automation.

---

## Onboarding & Usage

1. **Install dependencies:**

   ```bash
   ansible-galaxy collection install -r requirements.yml
   ```

1. **Customize variables as needed** by editing default_vars.yml for your desired lab topology, feature toggles, and credential patterns.

1. **Run create_instance.yml** for a full deploy/teardown cycle:

   ```bash
   ansible-playbook create_instance.yml
   ```

1. **Review per-playbook and per-template `.md` docs** for detailed task breakdowns, parameters, and troubleshooting information.
1. Use individual utility plays (provision_podman, provision_vbox, etc.) as demanded by your test or real-world scenario.

---

## Lab Patterns & Best Practices

- All files are mapped 1:1 to documentation in this directory
- Playbooks/templates should be updated together; documentation is maintained in sync
- Follow README-driven navigation to understand relationships, index, and workflow entrypoints

---
