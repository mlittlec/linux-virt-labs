# provision_vnc.yml Documentation

## Purpose & Overview

Installs, configures, and enables TigerVNC Server with GNOME desktop on OL lab VMs/servers for GUI and remote desktop access.

## When to Use

Use when you want to enable graphical (GNOME) interface and VNC-based graphical login for Oracle Linux hosts (VM or bare metal), useful for desktop/server testing.

## Key Variables & Inputs

- `vnc_port`: VNC port (display) to assign (per user).
- `vnc_default_password`: Password set for .vnc passwd.
- `username`: User to assign VNC login.
- `vnc_geometry`: VNC screen size (e.g., 1920x1080).

## Task Breakdown

- Installs "Server with GUI" package, TigerVNC server, and required modules.
- Sets system to graphical boot; configures vncserver systemd service for correct port.
- Updates vncserver.users and .vnc config files to assign geometry and session type.
- Prepares .vnc directories with correct ownership/permissions, generates password, and lock down access.
- Starts and enables the VNC service (systemd) using user-assigned service instance configuration.

## Dependencies & Relationships

- Consumed by create_instance.yml and other automated "desktop" setups.
- Can be coordinated with VirtualBox/KVM or remote workstation deployment.

## Output/Result
