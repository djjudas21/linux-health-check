# Linux Health Checks

This repo contains an Ansible playbook which executes various health checks on Linux.
This is a rapidly evolving project.

## Inventory

An Inventory is a list of hosts against which to execute the Playbook. This will be devised per
customer in a separate file. For testing, there is `inventory-localhost.ini` which executes against
your local system.

## Playbook

The playbook is a YAML file which contains tasks. We will configure one task to perform each test.

## Executing

The open-source version of Ansible is fine for these simple tests. Install with

```
# RHEL, Fedora, etc
sudo dnf install ansible

# Debian, Ubuntu, etc
sudo apt install ansible
```

With the local Ansible set up on the bastion, you can execute the playbook like this:

```
ansible-playbook -i inventory-localhost.ini playbook.yaml
```

Note that the default behaviour of Ansible means that hitting a failure in any task means the run
is halted. To work around this, this playbook makes use of `ignore_errors: true` so it can continue
running. This means that you must investigate any tests that are marked **ignored** at the end of
the run.
