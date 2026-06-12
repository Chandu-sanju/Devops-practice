# What is Ansible
  Ansible is an open source, a Configuration Management Tool and Deployment tool,
  =>  maintained by Redhat.
  => The main components of Ansible are playbooks, configuration management,deployment.
  =>  Ansible was written in Python.

### Configuration can be any task which we can perform on servers (hosts).
1) Install/Update/Uninstalling software (packages)
2) Copy files. Changing the permissions on the files or directories.
3) Start/Stop/ Restart the services
4) Creating / deleting the users  

### Various Configuration Management Tools
a) Ansible
b) Chef
c) Puppet
d) Salt Stack

### Ansible Features
    Ansible configure machines in an agent-less manner using SSH.
    Built on top of Python and hence provides a lot of Python’s functionality.
    YAML-Based Playbooks
    Uses SSH for secure connections.
    Follows Push based architecture for sending configurations.

### Push Based Vs Pull Based
    Tools like Puppet and Chef are pull based.
    Agents on the server periodically checks for the configuration information from central
    server (Master).
    Ansible is push based.
    Central server pushes the configuration information on target servers.
    You control when the changes are made on the servers.

## Push vs Pull Architecture
 PUSH MODEL (Ansible)
====================

        +------------------+
        |  Ansible Server  |
        +------------------+
           |      |      |
        SSH|   SSH|   SSH|
           v      v      v
      +------+ +------+ +------+
      |Host1 | |Host2 | |Host3 |
      +------+ +------+ +------+

Ansible pushes configurations to hosts when triggered.


PULL MODEL (Puppet/Chef)
========================

      +------------------+
      |  Master Server   |
      +------------------+
            ^      ^
            |      |
      Pull  |      | Pull
            |      |
      +------+ +------+
      |Host1 | |Host2 |
      +------+ +------+
            ^
            |
          Pull
            |
      +------+
      |Host3 |
      +------+

Agents periodically pull configurations from the Master.

## Ansible Architecture
![Ansible Architecture](https://raw.githubusercontent.com/Chandu-sanju/Devops-practice/main/Ansible/Images/Ansible-architecture.png)

## Ansible Components

<details>
<summary><b>1. Control Node</b></summary>

The machine where Ansible is installed and from which you run playbooks.

- Can be a laptop, workstation, or central automation server.
- Responsible for managing target hosts.
- Executes Ansible commands and playbooks.
- Uses SSH (Linux) or WinRM (Windows) to communicate with hosts.

> **Note:** Windows is generally not used as an Ansible Control Node.

</details>

<details>
<summary><b>2. Managed Nodes (Hosts)</b></summary>

These are the systems that Ansible manages.

Examples:
- Linux Servers
- Windows Servers
- Network Devices (Routers, Switches)
- Cloud Instances (AWS, Azure, GCP)

Ansible connects to managed nodes using:
- **SSH** for Linux/Unix systems
- **WinRM** for Windows systems

No agent installation is required on managed nodes.

</details>

<details>
<summary><b>3. Inventory</b></summary>

The Inventory contains the list of managed nodes.

Default Inventory Location:

```bash
/etc/ansible/hosts
```

### Types of Inventory

#### Static Inventory
A simple text file containing hostnames or IP addresses.

Example:

```ini
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
192.168.1.20
```

#### Dynamic Inventory
Automatically retrieves hosts from cloud providers such as:
- AWS
- Azure
- GCP

more info :- https://docs.ansible.com/projects/ansible/latest/inventory_guide/intro_inventory.html

Useful when servers are frequently created or destroyed.

</details>

<details>
<summary><b>4. Modules</b></summary>

Modules are small programs used by Ansible to perform tasks on managed nodes.

Common Modules:
- `yum`
- `apt`
- `service`
- `copy`
- `file`
- `user`

### Types of Modules

#### Core Modules
Maintained by the Ansible team.

#### Custom Modules
Created by users for specific requirements.

Example:

```yaml
- name: Install Nginx
  apt:
    name: nginx
    state: present
```

</details>

<details>
<summary><b>5. Playbooks</b></summary>

Playbooks are YAML files that define automation tasks.

They describe:
- What tasks to perform
- Which hosts to run on
- The desired state of the system

Example:

```yaml
---
- hosts: webservers
  become: yes

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```

Playbooks are the heart of Ansible automation.

</details>

<details>
<summary><b>6. Plugins</b></summary>

Plugins extend Ansible's functionality.

### Common Plugin Types

#### Connection Plugins
Handle communication with managed nodes.

Examples:
- SSH
- WinRM
- Docker

#### Callback Plugins
Control how output is displayed and logged.

Examples:
- Logging playbook results
- Formatting terminal output

#### Lookup Plugins
Retrieve data from external sources.

Examples:
- Files
- Environment Variables
- Databases

</details>

<details>
<summary><b>How Ansible Works</b></summary>

Ansible works by connecting to managed nodes and pushing small programs called **Ansible Modules** to perform tasks.

### Workflow

#### 1. Create Inventory
Define the hosts (IP addresses or hostnames) that Ansible will manage.

Example:

```ini
[webservers]
192.168.1.10
192.168.1.11
```

#### 2. Write a Playbook
Define the desired state of the target hosts in a YAML file.

Example:

```yaml
---
- hosts: webservers
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```

#### 3. Execute the Playbook
Run the playbook using:

```bash
ansible-playbook playbook.yml
```

#### 4. Connect & Push
Ansible connects to target hosts using:

- SSH (Linux/Unix)
- WinRM (Windows)

No agent installation is required on the managed nodes.

#### 5. Fact Gathering
Before executing tasks, Ansible collects system information known as **Facts**.

Examples:
- Hostname
- IP Address
- Operating System
- CPU Information
- Memory Information

These facts can be used inside playbooks for decision-making.

#### 6. Execute Modules
Ansible transfers and executes the required modules on the remote hosts.

Examples:
- `apt`
- `yum`
- `copy`
- `service`
- `user`

#### 7. Clean Up & Report
After execution:

- Temporary modules are removed.
- Results are reported back to the Control Node.

Possible Status Values:

| Status | Meaning |
|----------|----------|
| OK | No changes were required |
| Changed | Configuration was modified |
| Failed | Task execution failed |
| Skipped | Task was intentionally skipped |

### Summary Flow

```text
Inventory
    ↓
Playbook
    ↓
ansible-playbook
    ↓
SSH / WinRM Connection
    ↓
Fact Gathering
    ↓
Execute Modules
    ↓
Clean Up
    ↓
Status Report (OK / Changed / Failed)
```

</details>




================

## 📖 Sources

- GeeksforGeeks - Introduction to Ansible and its Architecture Components
- Official Ansible Documentation

## 🔗 Links

- https://www.geeksforgeeks.org/devops/introduction-to-ansible-and-its-architecture-components/
- https://docs.ansible.com/