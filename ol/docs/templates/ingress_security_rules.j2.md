# ingress_security_rules.j2 Documentation

## Purpose & Overview

Defines dynamic ingress rules for OCI security lists, rendered by Jinja2 template logic into Ansible variables used to control access to Oracle Linux cloud or lab VMs based on feature toggles (HAProxy, Nginx, NFS, HA Quay, PCP, OCFS2, iSCSI).

## When to Use

Use as include_vars in playbooks wherever you want to configure per-feature or per-service firewall policies in a data-driven, repeatable manner (see create_instance.yml, build.yml).

## Key Variables & Substitutions

- `use_haproxy`, `use_nginx`, `use_nfs`, `use_quay_ha`, `use_pcp`, `use_ocfs2`, `use_iscsi`: Control which ports/services are opened for which protocols (TCP/UDP).
- Always opens SSH (22/TCP) from the internet.
- Conditional entries expose rules for app, HTTP, HTTPS, NFS, database, high-availability, storage, or monitoring protocols.
- Jinja2 logic ensures only relevant entries are included in final rendered YAML.

## Dependencies & Relationships

- Used in concert with egress_security_rules.j2 for full security policy.
- Injected into OCI security list configuration via Ansible tasks during initial host, VM, or environment provisioning.
- Variables can be toggled in default_vars.yml or at runtime via ansible-playbook -e.

## Output/Result

- Renders a variable set instance_ingress_security_rules for application in OCI security lists, with explicit rule entries per enabled service.
