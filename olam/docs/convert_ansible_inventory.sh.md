# convert_ansible_inventory.sh Documentation

## Purpose & Overview
Bash shell script to convert a legacy or ad-hoc Ansible inventory file into a clustered, group-organized inventory format suitable for OLAM hybrid/cluster/topology use.

## When to Use
Use when migrating inventory files for new automation team members, onboarding multi-group labs (control, execution, hop, db), or standardizing disparate or hand-edited inventory lists.

## Key Inputs & Usage
- Takes a single argument: the inventory input file to convert (plain text)
- Outputs results to stdout (redirect or copy/paste output for use as new inventory)

## Script Breakdown
- Validates argument/input state and file existence.
- Processes input line-by-line to separate out control, execution, and other logical groups.
- Strips SSH connection settings for cleaner output.
- Emits per-group sections, with variables and peers set for clustering.
- Produces [all:vars] block for global user/SSH configuration.

## Dependencies & Relationships
- Consumed by onboarding, migration, or pipeline/init steps in OLAM labs.
- Called outside Ansible (CLI), but output is directly consumed as lab inventory.

## Output/Result
- Reusable, group-wise inventory file ready for Ansible playbooks or pipeline/distributed automation.
