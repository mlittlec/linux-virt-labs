# Deploy Oracle Cloud Native Environment 2

## Introduction

Oracle Cloud Native Environment (Oracle CNE) is a fully integrated suite for developing and managing cloud-native applications. The Kubernetes module is the core module. It deploys and manages containers and automatically installs and configures CRI-O, runC, and Kata Containers. CRI-O manages the container runtime for a Kubernetes cluster, which may be either runC or Kata Containers.

---

## Deploy Oracle CNE 2 Using These Ansible Playbooks

**Note:** If running in your own tenancy, read the `linux-virt-labs` GitHub project [README.md](https://github.com/oracle-devrel/linux-virt-labs) and complete the prerequisites before deploying the lab environment.

1. Open a terminal on the Luna Desktop.

1. Clone the `linux-virt-labs` GitHub project.

   ```shell
   git clone https://github.com/oracle-devrel/linux-virt-labs.git
   ```

1. Change into the working directory.

   ```shell
   cd linux-virt-labs/ocne2
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
      - A working KVM libvirt environment

      ```shell
      ansible-playbook create_instance.yml -e localhost_python_interpreter="/usr/bin/python3.6" -e ocne_type=libvirt
      ```

      This deploys a new Oracle Linux instance with a working libvirt environment ready for you to setup and install an Oracle CNE 2 install of your choice.

   b. Deploy a single Oracle Linux instance configured with the following:

      - Install of Oracle Cloud Native Environment (Oracle CNE)
      - A single control node and one worker node
      - A working KVM libvirt environment
  
      ```shell
      ansible-playbook create_instance.yml -e localhost_python_interpreter="/usr/bin/python3.6" -e install_ocne_rpm=true -e create_ocne_cluster=true -e "ocne_cluster_node_options='-n 1 -w 1'"
      ```

      If you want to use this for development together with Podman on the same instance add the `-e use_podman=true` parameter to the command as shown below:

      ```shell
      ansible-playbook create_instance.yml -e localhost_python_interpreter="/usr/bin/python3.6" -e use_podman=true -e install_ocne_rpm=true -e create_ocne_cluster=true -e "ocne_cluster_node_options='-n 1 -w 1'"
      ```

   c. The default values (see `defaults.yml`) can be overridden by creating a separate file with the revised definition to be created. This example deploys an Oracle Linux 9 (`os_version: 9`) instance with 8 CPUs (`instance_ocpus: 8`) and 128Mb memory (`instance_memory: 128`)  as well as increasing the boot volume size to be 256 Gb (`boot_volume_size_in_gbs: 256`).

      - i. Increase the Boot volume size, install libvirt, and use Oracle Linux 9.

         ```shell
         cat << EOF | tee instances.yml > /dev/null
         compute_instances:
         1:
            instance_name: "ocne"
            type: "server"
            instance_ocpus: 8
            instance_memory: 128
            boot_volume_size_in_gbs: 256
         ocne_type: "libvirt"
         install_ocne_rpm: true
         update_all: true
         os_version: "9"
         EOF
         ```

      - ii. Deploy a single Oracle Linux 9 instance configured using the custom configuration created in `instances.yml`:

         ```shell
         ansible-playbook create_instance.yml -e localhost_python_interpreter="/usr/bin/python3.6" -e "@instances.yml"
         ```

   **Note:** The free lab environment requires the extra variable `localhost_python_interpreter`, which sets `ansible_python_interpreter` for plays running on localhost. This variable is needed because the environment installs the RPM package for the Oracle Cloud Infrastructure SDK for Python, located under the python3.6 modules. Either remove, or update this variable to match your system's requirements.

   The default deployment shape uses the AMD CPU and Oracle Linux 8. To use an Intel CPU or Oracle Linux 9, add -e instance_shape="VM.Standard3.Flex" or -e os_version="9" to the deployment command.

   > **Important:** Wait for the playbook to run successfully and reach the pause task. At this stage of the playbook, the installation of Oracle Cloud Native Environment is complete, and the instances are ready. Take note of the previous play, which prints the public and private IP addresses of the nodes it deploys and any other deployment information needed while running the lab.

---

## Documentation Table

| File Name | Description |
| - | - |
| [block.md](./block.md) | Add block storage (volumes) to OCI compute instances |
| [build.md](./build.md) | Launch and configure compute instances in OCI |
| [create_fss.md](./create_fss.md) | Provision OCI File Storage Service (FSS) components |
| [create_instance.md](./create_instance.md) | **Main Orchestration:** Provisions all resources and runs end-to-end lab automation |
| [default_vars.md](./default_vars.md) | Defines structure, sizing, cluster, user, and feature toggle variables used by all playbooks. |
| [delete_fss.md](./delete_fss.md) | Teardown (delete) FSS components cleanly |
| [deploy_ocne_libvirt.md](./deploy_ocne_libvirt.md) | Deploy OCNE cluster on local KVM/Libvirt |
| [deploy_ocne_oci.md](./depploy_ocne_oci.md) | Deploy OCNE cluster directly on OCI infrastructure |
| [deploy_ocne_none.md](./deploy_ocne_none.md) | No-operation placeholder for skipping cluster setup |
| [fss_deployments.md](./fss_deployments.md) | Generates Kubernetes manifests for FSS-backed PersistentVolume/Pod |
| [host_setup.md](./host_setup.md) | Prepares OS/user/SSH/facts on each instance |
| [provision_podman.md](./provision_podmam.md) | Install Podman and container tools for OCI/Linux |
| [requirements.md](./requirements.md) | Ansible Galaxy collections needed for all playbooks |
| [update_all_rpms.md](./update_all_rpms.md) | Update all OS packages and reboot as needed |

---

## Jinja2 Templates

| Template | Purpose / Usage |
| --- | --- |
| [egress_security_rules.j2.md](./templates/egress_security_reules.j2.md) | Jinja2 template for outbound network security rules (exported as YAML var file) |
| [ingress_security_rules.j2.md](./templates/ingress_security_rules.j2.md) | Jinja2 template for inbound network security rules (exported as YAML var file) |
| [fss_pv.j2.md](./templates/fss_pv.j2.md) | Template for Kubernetes PersistentVolume integrating OCI FSS. |
| [fss_pvc.j2.md](./templates/fss_pvc.j2.md) | Template for Kubernetes PersistentVolumeClaim (PVC) for FSS |
| [fss_pod.j2.md](./templates/fss_pod.j2.md) | Template for sample/test K8s Pod using the FSS PVC |
| [fss_vars.j2.md](./templates/fss_vars.j2.md) | Template for storing FSS variable output during provisioning |
| [oci_vars.j2.md](./templates/oci_vars.j2.md) | Template for storing key OCI variables as a YAML snippet |

---

## Tree Listing

```markdown
├── ocne2
    ├── block.md                          # Add block storage (volumes) to OCI compute instances
    ├── build.md                          # Launch and configure compute instances in OCI
    ├── create_fss.md                     # Provision OCI File Storage Service (FSS) components
    ├── create_instance.md                # Main Orchestration Playbook: Provisions all resources and runs end-to-end lab automation
    ├── default_vars.md                   # Defines structure, sizing, cluster, user, and feature toggle variables used by all playbooks.
    ├── delete_fss.md                     # Teardown (delete) FSS components cleanly
    ├── deploy_ocne_libvirt.md            # Deploy Oracle CNE cluster on local KVM/Libvirt
    ├── deploy_ocne_none.md               # Configures an OCI Instance ready to Deploy Oracle CNE cluster
    ├── deploy_ocne_oci.md                # Deploy Oracle CNE cluster directly on OCI infrastructure
    ├── fss_deployments.md                # Generates Kubernetes manifests for FSS-backed PersistentVolume/Pod
    ├── host_setup.md                     # Prepares OS/user/SSH/facts on each instance
    ├── provision_podman.md               # Install Podman and container tools for OCI/Linux
    ├── README.md                         # This File
    ├── requirements.md                   # Ansible Galaxy collections needed for all playbooks
    ├── update_all_rpms.md                # Update all OS packages and reboot as needed
    └── templates
        ├── egress_security_rules.j2.md   # Jinja2 template for outbound network security rules (exported as YAML var file)
        ├── fss_pod.j2.md                 # Template for sample/test K8s Pod using the FSS PVC
        ├── fss_pv.j2.md                  # Template for Kubernetes PersistentVolume integrating OCI FSS.
        ├── fss_pvc.j2.md                 # Template for Kubernetes PersistentVolumeClaim (PVC) for FSS
        ├── fss_vars.j2.md                # Template for storing FSS variable output during provisioning
        ├── ingress_security_rules.j2.md  # Jinja2 template for inbound network security rules (exported as YAML var file)
        └── oci_vars.j2.md                # Template for storing key OCI variables as a YAML snippet
```

---

## Relationships and Data Flow

- **create_instance.yml** triggers almost all other playbooks and templates.
- **build.yml** → **block.yml** (for volume attachment)
- **create_fss.yml** → **fss_vars.j2** (creates variables for storage/manifest templates)
- **fss_deployments.yml** utilizes **fss_pv.j2**, **fss_pvc.j2**, **fss_pod.j2**
- **deploy_*.yml** (libvirt/oci/none) drive lab or cluster setup per deployment type.
- **host_setup.yml** is invoked for each instance as part of setup.
- Security rules are templated by **ingress_security_rules.j2** and **egress_security_rules.j2**, consumed whenever a new subnet or security list is created.

---

## Tips and Best Practices

- Review each individual Markdown doc for file-specific details, usage notes, and in-context examples.
- Always use version-pinned dependency management via `requirements.yml`.
- Keep code and docs in sync: update the corresponding `.md` file for any playbook, role, or template change.

---

## References

- [Oracle Cloud Infrastructure Documentation](https://docs.oracle.com/en-us/iaas/Content/home.htm)
- [OCI Ansible Collection](https://docs.oracle.com/en-us/iaas/tools/oci-ansible-collection/)
- [Oracle Cloud Native Environment](https://docs.oracle.com/en/solutions/deploy-cloud-native-environments/index.html)
- [Kubernetes Docs](https://kubernetes.io/docs/)
- [Podman Docs](https://podman.io/)

---

For troubleshooting, edge use cases, or advanced onboarding, see individual `.md` files in this directory.
