# toolbox.j2 Template Documentation

## Purpose

Defines a Kubernetes Deployment for running the Rook Ceph toolbox pod, which equips the cluster with a pre-built CLI toolset for Ceph management, troubleshooting, and operational actions.

## How the Template is Used

- Deployed by ceph_deployments.yml to every control plane node as toolbox.yaml.
- Provides on-demand access to ceph/rados/rook commands within the cluster, using real-time config and keyring data for secure admin operations.

## Key Variables & Substitutions

- No dynamic Jinja2 substitution; configuration is static apart from container runtime environment/secret/ConfigMap bindings.

## Rendered Example (abridged)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rook-ceph-tools
  namespace: rook
spec:
  replicas: 1
  template:
    spec:
      containers:
      - name: rook-ceph-tools
        image: container-registry.oracle.com/olcne/ceph:v17.2.5
        command:
          - /bin/bash
          - -c
          - |
            # Inline ceph config/keyring and endpoint update script (see template for details)
        env:
          - name: ROOK_CEPH_USERNAME
            valueFrom:
              secretKeyRef:
                name: rook-ceph-mon
                key: ceph-username
        volumeMounts:
          - mountPath: /etc/ceph
            name: ceph-config
          - name: mon-endpoint-volume
            mountPath: /etc/rook
          - name: ceph-admin-secret
            mountPath: /var/lib/rook-ceph-mon
            readOnly: true
      volumes:
        - name: ceph-admin-secret
          secret:
            secretName: rook-ceph-mon
            optional: false
            items:
              - key: ceph-secret
                path: secret.keyring
        - name: mon-endpoint-volume
          configMap:
            name: rook-ceph-mon-endpoints
            items:
              - key: data
                path: mon-endpoints
        - name: ceph-config
          emptyDir: {}
```

## Special Logic

- Startup script replicates toolbox.sh logic inline for endpoint/keyring/config file management.
- Watches and updates ceph.conf live as MON endpoints or failovers occur.
- Reads secrets from env or files for keyring population; maintains secure (non-root) context.
- Tolerates brief node unreachable status for resilience.

## Related Files

- Rendered by: ceph_deployments.yml.
- Used with: cluster.j2, filesystem.j2, storageclass.j2, providing a CLI for CephFS and cluster/storage troubleshooting.
