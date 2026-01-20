# istio_lb.j2 Template Documentation

## Purpose

Defines a YAML configuration fragment to customize the behavior of the Istio ingress gateway LoadBalancer for Oracle Cloud Infrastructure (OCI) Kubernetes clusters.

## How the Template is Used

- Rendered by provision_istio.yml and used as the profile input for `olcnectl module create` during Istio installation.
- Applies annotation-based configuration to the Istio ingressgateway Service to control load balancer characteristics in OCI.
- Ensures the Istio LB is provisioned as internal, flexible, and not security-list managed.

## Key Variables & Substitutions

- This template is static (no Jinja2 substitutions), encoding all required annotations.

## Rendered Example

```yaml
components:
  ingressGateways:
  - name: istio-ingressgateway
    k8s:
      serviceAnnotations:
        service.beta.kubernetes.io/oci-load-balancer-security-list-management-mode: "None"
        service.beta.kubernetes.io/oci-load-balancer-internal: "true"
        service.beta.kubernetes.io/oci-load-balancer-shape: "flexible"
        service.beta.kubernetes.io/oci-load-balancer-shape-flex-min: "10"
        service.beta.kubernetes.io/oci-load-balancer-shape-flex-max: "10"
```

## Special Logic

- Configures the ingress gateway load balancer as INTERNAL-ONLY (not public-facing) with fixed bandwidth (via min/max).
- Disables OCI's security-list management for the LB (lets user handle via automation/explicit rules).

## Related Files

- Written and referenced from: provision_istio.yml.
- Intended for: Service annotations, customizing load balancer resources when using Istio on Oracle Kubernetes.
