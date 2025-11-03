# AnsaibleUserManagement
 ### Automate user management, group access, and software installation across multiple Linux servers using Ansible.
 <img width="891" height="452" alt="image" src="https://github.com/user-attachments/assets/bbe53837-86f9-42f4-817a-7b95081611a5" />
 <img width="1920" height="899" alt="image" src="https://github.com/user-attachments/assets/c26d7957-2c10-4e56-b7a7-5e315c2315cf" />
 <img width="1917" height="980" alt="image" src="https://github.com/user-attachments/assets/d684c503-cc13-40eb-8277-cc895ebecb5e" />
<img width="1917" height="865" alt="image" src="https://github.com/user-attachments/assets/d305af9d-1b73-44a5-a285-167876bd60a8" />




---
## Project Overview

This project automates the creation, configuration, and deletion of users and groups, as well as installation of essential software on multiple servers.

It’s designed using modular Ansible roles, allowing fine-grained control over:

 - Which users exist on which servers
  
 - Which software packages are installed
  
 - Which groups get created or removed automatically
 ---
## Project Features

  - ✅ Automated user creation & deletion
  - ✅ Automated group management (create/remove)
  - ✅ Automatic SSH key setup
  - ✅ Sudo privileges configuration
  - ✅ Software installation per server/group
  - ✅ Support for different users per environment using group_vars and host_vars
  - ✅ Idempotent — safely re-runnable anytime
  - ✅ Works on Ubuntu / Debian / CentOS

---
## Folder Structure
```
user-management/
│
├── inventories/
│   └── hosts.ini               # Target servers list
│
├── group_vars/
│   ├── all.yml                 # Common users & packages
│   ├── web_servers.yml         # Users & packages for web servers
│   ├── db_servers.yml          # Users & packages for DB servers
│   └── cache_servers.yml       # Users & packages for cache servers
│
├── host_vars/
│   └── node3.yml               # Host-specific overrides (optional)
│
├── roles/
│   ├── users/
│   │   ├── tasks/
│   │   │   └── main.yml        # User & group management logic
│   │   └── files/
│   │       └── authorized_keys/
│   │           ├── devops.pub
│   │           ├── webadmin.pub
│   │           └── dbadmin.pub
│   │
│   └── software/
│       └── tasks/
│           └── main.yml        # Software installation logic
│
└── playbook.yml                # Main Ansible playbook

```

---
## Configuration
1️ Define Inventory

#### Edit inventories/hosts.ini to list your servers:
```
[web_servers]
web1 ansible_host=192.168.1.10 ansible_user=ubuntu
web2 ansible_host=192.168.1.11 ansible_user=ubuntu

[db_servers]
db1 ansible_host=192.168.1.12 ansible_user=ubuntu

```

2️ Define Users and Software

#### Example: group_vars/web_servers.yml
```
users:
  - name: webadmin
    groups: ["nginx", "developers"]
    state: present
    sudo: true
    ssh_key: webadmin.pub

software_packages:
  - nginx
  - git
  - curl

```

3️ Run the Playbook

#### Execute all roles:
```
ansible-playbook -i inventories/hosts.ini playbook.yml

```
#### Run only software installation:
```
ansible-playbook -i inventories/hosts.ini playbook.yml --tags software

```
#### Run only user management:
```
ansible-playbook -i inventories/hosts.ini playbook.yml --tags users

```
---
## Key Roles Explained
## 1 users

Manages all user-related tasks:

  - Creates users and groups
  
  - Deploys SSH keys
  
  - Configures sudo access

Removes old users and unused groups

## 2 software

Installs and updates packages listed in software_packages:

  - Supports multiple Linux distributions
  
  - Can be extended to include Docker, Jenkins, etc.

---
## How It Works (Flow)

- Inventory lists target servers

- group_vars / host_vars define which users and software each server needs

- Playbook calls the roles in order:

  - software (installs tools)
  
  - users (creates users and groups)

- Ansible applies changes idempotently
---
