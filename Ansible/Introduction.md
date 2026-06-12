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