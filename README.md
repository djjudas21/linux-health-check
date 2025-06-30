# Linux Health Checks

This repo contains an Ansible playbook which executes various health checks on Linux.
This is a rapidly evolving project.

## Inventory

An Ansible Inventory is a list of hosts against which to execute the Playbook. This will be devised per
customer in a separate file, e.g. `inventory-mycorp.ini`.
For testing, there is `inventory-localhost.ini` which executes against
your local host (only effective on Linux).

For more details, see
[How to build your inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html).

## Prerequisites

To run these checks, you will require:

- a system capable of running Ansible, preferably Linux
- passwordless SSH access to all target hosts
- passwordless sudo access (only needed for some checks)

Once the customer has set up user accounts on all target hosts, SSH to it from your Ansible host to ensure it works. If it prompts for a password, set up passwordless SSH as follows:

1. Ensure you have private & public keys on the Ansible host (check for `~/.ssh/id_rsa`)
2. If you don't, generate a new keypair with `ssh-keygen`
3. Copy the public key to all target hosts with `ssh-copy-id -i ~/.ssh/id_rsa user@host`

Passwordless sudo can be configured by adding a line like this to `/etc/sudoers`. This will probably need to be done by the customer.

```
myuser ALL=(ALL) NOPASSWD: ALL
```

## Installation

The open-source version of Ansible is fine for these simple tests. Install it on the system you will use to run the tests from with:

```
# RHEL, Fedora, etc
sudo dnf install ansible

# Debian, Ubuntu, etc
sudo apt install ansible
```

Most of the checks can be executed on the target systems using only the standard system utilities.
However, some checks require extra tools. These can be installed by running the `install_deps.yaml` playbook.

```
ansible-playbook -i inventory-localhost.ini install_deps.yaml
```

## Executing

With the local Ansible set up on the bastion, passwordless SSH working, and deps installed, you can execute the playbook like this:

```
ansible-playbook -i inventory-localhost.ini playbook.yaml
```

Note that the default behaviour of Ansible means that hitting a failure in any task means the run
is halted. To work around this, this playbook makes use of `ignore_errors: true` so it can continue
running. This means that you must investigate any tests that are marked **ignored** at the end of
the run.

Currently, the method of collecting & collating results is manual.