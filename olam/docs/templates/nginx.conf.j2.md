# nginx.conf.j2 Documentation

## Purpose & Overview

Feature-conditional Nginx configuration template for Oracle Linux Automation Manager labs. Renders a full Nginx config for web endpoints and HAProxy health checks, with modular toggles for HTTP and API endpoints.

## When to Use

Render the config when enabling Nginx for PAH or other web/app/health endpoints. Best for CI/CD, lab demos, or infrastructure onboarding scenarios needing HTTP(s) and service health endpoints.

## Key Variables & Inputs

- use_haproxy: Determines whether the 8080/health and /nginx-health server block is included.
- All other config is static or distributed default.

## Task Breakdown

- Base config is provided for Nginx logs, mime types, includes, and worker processes.
- If use_haproxy is true, exposes /nginx-health and /health endpoints for external requests on 8080.
- All configs are best practices derived from Oracle Linux Automation Manager lab experience.

## Dependencies & Relationships

- Used by provision_pah.yml and other playbooks that set up web endpoints.
- Should be paired with firewall and host provisioning for correct enablement.

## Output/Result

- Nginx configuration file ready for GUI/web service, health check, and API lab usage.
