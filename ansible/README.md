# Ansible Docker & Jenkins configuring Project

This Ansible Galaxy project configures a Docker host and Jenkins server on an Ubuntu system using roles.
 The playbook `Playbook.yaml` is designed to be run with tags and conditionally checks if the operating system is Ubuntu.

## Requirements

- Ansible 2.9+
- Ubuntu 18.04/20.04/22.04
- Ansible Vault for encrypted variables

## Usage

To run the playbook, use the following command:

```bash
ansible-playbook Playbook.yaml --ask-vault-pass
```

### Tags

The playbook supports the following tags:

- `docker`: Configure Docker host
- `jenkins`: Configure Jenkins server

You can run the playbook with specific tags using the `--tags` option. For example, to only configure Docker, use:

```bash
ansible-playbook Playbook.yaml --ask-vault-pass --tags docker
```

## Playbook Structure

The `Playbook.yaml` file uses roles to organize the configuration tasks. The roles are defined as follows:

- **docker**: Contains tasks for installing and configuring Docker.
- **jenkins**: Contains tasks for installing and configuring Jenkins.

### Example Playbook

```yaml
---
- name: Configure Docker and Jenkins on Ubuntu
  hosts: all
  become: yes

  vars_files:
    - vault.yml

  pre_tasks:
    - name: Update apt package index
      apt:
      update_cache: yes

  roles:
    - { role: docker, when: ansible_distribution == "Ubuntu", tags: docker }
    - { role: jenkins, when: ansible_distribution == "Ubuntu", tags: jenkins }
```

## Role Structure

Each role follows the standard Ansible role structure:

```

  docker/
    tasks/
      main.yml
  jenkins/
    tasks/
      main.yml
```

### Docker Role (`docker/tasks/main.yml`)

- Install Docker
- Start and enable Docker service


### Jenkins Role (`jenkins/tasks/main.yml`)

- Install Jenkins
- Start and enable Jenkins service
- Install Jenkins Plugins

## Variables

Sensitive variables, such as Jenkins admin passwords, should be stored in an encrypted Ansible Vault file (`vault.yml`).
Ensure you have created this file and added the necessary variables.

### Creating and Editing Vault File

To create a vault file:

```bash
ansible-vault create vault.yml
```

To edit the vault file:

```bash
ansible-vault edit vault.yml
```

## Author

Ahmed Aboraya
```
