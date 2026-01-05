# olam-hello-world.yaml Documentation

## Purpose & Overview

A minimal Ansible playbook demonstrating the structure, syntax, and basic use of Oracle Linux Automation Manager (OLAM). Prints a "Hello World" message for onboarding, classroom, or demonstration environments.

## When to Use

Use this playbook as:

- A starting point for new users learning how Oracle Linux Automation Manager/Ansible works
- A smoke-test to verify basic Ansible/Oracle Linux Automation Manager installation and task execution
- A demonstration of Ansible syntax, play/host/task structure

## Key Variables & Inputs

- Runs on: localhost
- No variables required
- No input or user interaction expected

## Task Breakdown

- Defines a single play
- Includes a single task: prints "Hello! Welcome to Oracle Linux Automation Manager." using ansible.builtin.debug

## Dependencies & Relationships

- Standalone, does not require any other roles, modules, or dependencies outside Ansible core.
- Can be extended with tasks/roles as experience grows.

## Output/Result
