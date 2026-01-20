# provision_vnc.yml Documentation

## Purpose & Overview

Installs and configures GNOME and TigerVNC server for engine nodes in Oracle Linux Virtualization Manager, providing graphical access for admin, troubleshooting, or desktop/desktop-automation use.

## When to Use

Run after engine host provisioning, when GUI login or remote desktop access is required for setup, demos, or advanced debugging in virtual lab infrastructure.

## Key Variables & Inputs

- Hosts: engine
- Username, password, geometry, and VNC port definition.

## Task Breakdown

- Installs all packages required for graphical sessions and VNC (Server with GUI, tigervnc, etc.)
- Sets the system to default to graphical.target.
- Creates the user’s .vnc directory and a secure password file.
- Configures session (geometry, GNOME) and enables a systemd VNC service for runtime access.

## Dependencies & Relationships

- Called after classic host configuration
- May be referenced by GUI automation, test, or remote onboarding processes

## Output/Result
