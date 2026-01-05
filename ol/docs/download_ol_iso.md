# download_ol_iso.yml Documentation

## Purpose & Overview

Automates the download of Oracle Linux ISO images for local VM creation or bare-metal installation, using the Oracle public ISO repository.

## When to Use

Use to programmatically fetch the required Oracle Linux ISO (specified by version and update) on your lab host for KVM, VirtualBox, or physical machine builds.

## Key Variables & Inputs

- `ol_iso_version`: Oracle Linux major version (e.g., 8, 9, 10).
- `ol_update`: Update level to fetch (e.g., 4, 10).
- `username`: Target user for file ownership.
- `base_url`: The Oracle download base URL, default: https://yum.oracle.com/ISOS/OracleLinux (overridable)
- `vbox`/`server`: Host(s) that will perform the download.

## Task Breakdown

- Constructs the exact ISO URL based on version/update.
- Downloads the ISO to `/home/{{ username }}` on the target host.
- Ensures download is complete (retries up to 5 times), and sets file permissions.
- Registers and checks result for success (supports repeated or interrupted runs).

## Dependencies & Relationships

- Uses variables from default_vars.yml.
- Downstream requirement for VM/KVM installs needing local ISO image.
- Can share ISOs between roles/VM definitions for reproducibility.

## Output/Result

- Oracle Linux ISO image is present and ready for VM or bare-metal installation flows.
