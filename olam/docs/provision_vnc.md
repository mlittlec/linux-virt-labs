# provision_vnc.yml Documentation

## Purpose & Overview

Installs and configures TigerVNC server and GNOME desktop for lab/dev automation hosts, enabling remote graphical logins for CLI alternative, UI tests, or onboarding.

## When to Use

Run on devops-node or as part of full-lab automation to provide GUI access for demos, debugging, or student/QA use.

## Key Variables & Inputs

- Hosts: devops-node
- `vnc_port`, `vnc_default_password`, `username`, `usergroup`, `vnc_geometry`

## Task Breakdown

- Installs GUI and VNC components.
- Sets system to default to graphical login.
- Creates the user's .vnc directory and password file with secure permissions.
- Configures session geometry, assigns username to port, and sets up systemd VNC service.
- Auth/ownership/permissions configured per-user for secure multi-user access.

## Dependencies & Relationships

- Runs after initial host configuration (host_setup, provision_instance_basics).
- Used by devops teams for remote lab access.

## Output/Result
