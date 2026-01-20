# vm.j2 Template Documentation

## Purpose

Defines a KubeVirt VirtualMachine intended for the OCNE lab/demo stack, combining OCI container images, persistent (CephFS) volumes, and cloud-init for user and SSH provisioning.

## How the Template is Used

- Deployed as part of ceph_deployments.yml (vm.yaml).
- Demonstrates best-practice pattern for creating VMs attached to dynamically provisioned CephFS-backed volumes.
- Integrates cloud-native OCI registry, persistent storage, and cloud-init initialization in a single K8s manifest.

## Key Variables & Substitutions

- All config is static; no Jinja2 variables.

## Rendered Example (abridged)

```yaml
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  name: ol9-nocloud
spec:
  running: true
  template:
    spec:
      terminationGracePeriodSeconds: 30
      networks:
      - name: foo
        pod: {}
      domain:
        cpu:
          cores: 2
          sockets: 1
          threads: 1
        resources:
          requests:
            memory: 2048M
        devices:
          interfaces:
          - name: foo
            masquerade: {}
            ports:
            - port: 80
            - port: 22
          disks:
          - name: containerdisk
            disk:
              bus: virtio
          - name: mypvcdisk
            disk:
              bus: virtio
          - name: cloudinitdisk
            disk:
              bus: virtio
      volumes:
      - name: containerdisk
        containerDisk:
          image: fra.ocir.io/OCIR_NAMESPACE/demo/oraclelinux-cloud:9.4-terminal
          imagePullSecret: ocirsecret
      - name: mypvcdisk
        persistentVolumeClaim:
          claimName: cephfs-pvc
      - name: cloudinitdisk
        cloudInitNoCloud:
          secretRef:
            name: vmi-userdata-secret
```

## Special Logic

- Combines OCI Registry image, CephFS volume, and cloud-init for rapid, automated VM onboarding and persistence.
- Exposes network ports for SSH and HTTP; tolerates native K8s pod networking.
- Termination/cleanup resilience with controlled grace period.

## Related Files

- Rendered by: ceph_deployments.yml as vm.yaml.
- Consumed with: pvc.j2, storageclass.j2, cloud-init secrets, demonstrating full lab provisioning path.
