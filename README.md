# AnsaibleUserManagement
 ### Automate user management, group access, and software installation across multiple Linux servers using Ansible.
---
## Project Overview

This project demonstrates how to use Ansible to automate system administration tasks across multiple servers.
It manages:

 - User and group creation/removal

 - Application installation (like nginx, git, postgresql, etc.)

 - Separate configurations for web servers and database servers

The setup includes 5 servers:

- 🖥️ 3 × web_server

- 🗄️ 2 × db_server
 <img width="891" height="452" alt="image" src="https://github.com/user-attachments/assets/bbe53837-86f9-42f4-817a-7b95081611a5" />

- All configurations are automated through Ansible Playbooks and Roles to ensure consistency, repeatability, and zero manual errors.
 <img width="1917" height="865" alt="image" src="https://github.com/user-attachments/assets/d305af9d-1b73-44a5-a285-167876bd60a8" />
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
ansible-user-group/
├── inventory.ini
├── playbook.yml
├── group_vars/
│   ├── web_server.yml
│   └── db_server.yml
└── roles/
    ├── user_management/
    │   └── tasks/main.yml
    └── install_app/
        └── tasks/main.yml

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

#### Example: group_vars/db_servers.yml
```
groups_to_create:
  - name: dbadmins # Database administration group
    state: present

  - name: dbbackup # Database backup group
    state: present

users_to_create:
  - name: arjun # DB admin user
    group: dbadmins
    state: absent

  - name: amit # DB backup user
    group: dbbackup
    state: absent
    
apps_to_install:
  - postgresql
  - python3-psycopg2

```

3️ Run the Playbook

#### Execute all roles:
```
ansible-playbook -i inventory.ini playbook.yml

```
<img width="1917" height="873" alt="image" src="https://github.com/user-attachments/assets/f3dbad54-5e52-4e57-86a5-bbff1bf9d559" />

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

## How to Run the Project
### Step 1: Clone the Repository
```
https://github.com/MoizAnsari-Dev/AnsaibleUserManagement.git
cd AnsaibleUserManagement
```
### Step 2: Test Connection
```
ansible all -m ping -i inventory.ini
```
### Step 3: Create Users & Groups
```
ansible-playbook playbooks/create_user_group.yml -i inventory.ini
```
### Step 4: Install Applications
```
ansible-playbook playbooks/install_app.yml -i inventory.ini
```
### Step 5: Remove Users & Groups
```
ansible-playbook playbooks/remove_user_group.yml -i inventory.ini
```
## 🏆 Results

- Reduced manual configuration time by 90%

- Ensured consistent and reproducible setups

- Demonstrated Ansible best practices using roles and group_vars
