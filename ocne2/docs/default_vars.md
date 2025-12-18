# default_vars.yml — Documentation

## Purpose

Defines all primary variables for playbook-driven creation, configuration, and deployment of resources in Oracle Cloud Infrastructure (OCI) and for the Oracle Cloud Native Environment (Oracle CNE) stack. These defaults control instance shapes, user access, block storage, cluster provisioning, and feature toggles.

## Variable Breakdown

### Compute Instance Parameters

- **compute_instances:**  
  Dictionary mapping instance numbers to specifications:
  - `instance_name`: Name of the VM (e.g. `"ocne"`, `"ol-node-01"`, etc.)
  - `type`: Role or group (e.g. `"server"` or `"client"`)
  - `boot_volume_size_in_gbs`: Size of the boot volume (GB)

- **os / os_version:**  
  Determines the Oracle Linux version used by the deployed compute instance. Valid values are: `"8"`, `"9"` or `"10"`

- **instance_shape / instance_ocpus / instance_memory:**  
  OCI shape specifiers (shape name (`instance_shape:`), vCPUs (`instance_cpus:`), memory in GB (`instance_memory:`), etc.).

- **`subnet_cidr_block:`**  
  CIDR for the subnet into which VMs are placed.

### User and Access

- **`username:`**: Default user ("oracle")
- **`usergroup:`**: User's group (defaults to username)
- **`user_default_password:`**: Initial password for the user
- **`debug_enabled:`**: Toggle for additional debug output

### Block Storage Controls

- **`add_block_storage:`** Enable/disable extra block volume addition
- **`block_volume_size_in_gbs:`** Size per block volume
- **`block_count:`** Number of block volumes to attach

### Networking and Load Balancing

- **`use_ingress_lb:`** Enable/disable use of an ingress load balancer

### OCNE Cluster and Features

- **`ocne_type:`** Cluster type for Oracle CNE deployment (e.g. "libvirt")
- **`install_ocne_rpm:`** Install Oracle CNE RPM packages if set true
- **`create_ocne_cluster:`** Toggle overall Oracle CNE cluster creation
- **`create_ocne_oci_cluster:`** Toggle creation of an Oracle CNE cluster in OCI
- **`debug_oci_cluster:`** Enable debugging for cluster creation
- **`ocne_cluster_node_options:`** Additional CLI node options
- **`ocne_cluster_name:`** Name assigned to the OCI cluster
- **`num_cp_nodes:` / `num_wk_nodes:`** Number of Control-plane and Worker nodes

### Other Feature Flags

- **`update_all:`** Toggle for updating all packages with RPMs
- **`use_fss:`** Enable/disable FSS (File Storage Service) provisioning
- **`use_podman:`** Enable/disable Podman/container tooling installation

### Miscellaneous

- **`oci_yum_region:`** (Commented) Optionally specifies the regional YUM repository for package management

## Notes

- Toggling these variables enables or disables major playbook features and deployment flows.
- These defaults can be overridden via vars_files or set directly on the command line.
- Designed for use across all playbooks in the `ocne2` automation stack.

