# Ansible Installtion

Ansible installtion is easy process..

<details>
<summary><b>How to install</b></summary>

Command:- yum install ansible or dnf install ansible (latest)
```
 Ansible uses the Python interpreter to execute its code,Without Python, Ansible cannot run on the Control Node.
 On RHEL/CentOS systems, python-pip and ansible are available via EPEL REPOSITORY if not --
   Install python
 $ sudo yum install python3 -y
 # if python poiting to python version2 then we make it point to python3
 $ sudo alternatives --set python /usr/bin/python
  => # If you're on RHEL/CentOS 6:
    $ rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/\
    epel-release-6-8.noarch.rpm
    # If you're on RHEL/CentOS 7:
    $ yum install epel-release
    # If you're on RHEL 8+/Fedora:
    $ dnf install epel-release
```

### Command:- pip install ansible # IF python version 2 install in your machine

### Command:- pip3 install ansible  # Through python 3

> [!IMPORTANT]
> If you install anisble through pip /etc/ansible/hosts (default inverntory file) won't create
> mkdir /etc/ansible
> vi /etc/ansible/hosts/ add ip addr

</details>

<details>
<summary><b>Ansible User vs Root User</b></summary>

Ansible User vs Root User

> [!NOTE]
> While learning Ansible, you may see two approaches:
> 1. Using the **root** user
> 2. Using a dedicated **ansible** user

## Using Root User

```text
Control Node  : root
Managed Nodes : root
Authentication: SSH Key
```

### Advantages

- Easy to set up
- Good for beginners
- Useful for lab environments

### Disadvantages

- Not recommended for production
- Less secure
- No user-level auditing

---

## Using Ansible User (Recommended)

```text
Control Node  : ansible
Managed Nodes : ansible
Authentication: SSH Key
Privileges    : sudo (become: yes)
```

Example:

```yaml
---
- hosts: all
  become: yes
```

### Advantages

- More secure
- Industry best practice
- Common in production environments
- Preferred in interviews

---

> [!TIP]
> If you are new to Ansible, start with the **root user** to learn the basics quickly.

> [!IMPORTANT]
> For real-world projects and production environments, use a dedicated **ansible** user with **sudo privileges** and **SSH key authentication**.

## My Recommendation

```text
Learning / Home Lab  → Use root
Production / Company → Use ansible + become: yes
```
</details>
<details>
<summary><b>SSH Generation and Copy to Managed Node </b></summary>

### Command => ssh-keygen
Proceed with enter enter => with out selecting prefix

Where you can find the pub and private key => from which profile you run , you can find there home direcotry
for suppose root => cd /root/.ssh # you can see
if anisble user => cd /home/ansible/.ssh # You can see

### Copy to managed node
Command
ssh-copy-id username@target_ip
What does ssh-copy-id do?

The ssh-copy-id command automatically:

Connects to the target machine
Creates the .ssh directory if it does not exist
Creates the authorized_keys file if it does not exist
Copies your public key (id_rsa.pub, id_ed25519.pub, etc.) to the target machine
Appends the public key to the authorized_keys file
Sets the correct permissions on .ssh and authorized_keys

[!TIP]
ssh-copy-id is the easiest and recommended method for configuring passwordless SSH authentication.

Manual Equivalent

If ssh-copy-id is not available, perform the following steps on the target machine:

mkdir -p ~/.ssh
chmod 700 ~/.ssh

vi ~/.ssh/authorized_keys
# Paste public key here

chmod 600 ~/.ssh/authorized_keys
User-Specific Authentication

SSH key authentication is user-specific, not machine-specific.

Login as Root
ssh root@server1

Public key location:

/root/.ssh/authorized_keys
Login as Chandu
ssh chandu@server1

Public key location:

/home/chandu/.ssh/authorized_keys


[!IMPORTANT]
The public key must be placed in the authorized_keys file of the same user you intend to SSH into.
# ✅ Ansible Setup Complete!

## Congratulations! 🎉

We have successfully set up **Ansible** and are ready to automate your infrastructure.
</details>



