## What is Ad hoc command ?
```
  An ad hoc command is a one-time Ansible command used to perform a task on one or more managed hosts without creating a playbook.
  Simple we can say - re usable tasks or on fly commands to excute on managed nodes

    Use it for quick operations such as:
    ==================================
    Checking connectivity
    Rebooting servers
    Running Linux commands
    Copying files
    Installing packages
    Managing users
```
# Basic Syntax: ansible <host-pattern> -m <module> -a "<arguments>"  
                ansible  -i /tmp/Wipro/USA -m shell -a "ls /tmp|grep"

# Rebooting servers
The default module for the ansible command-line utility is the ansible.builtin.command module. 
```
    ### ansible atlanta -a "/sbin/reboot"
    So you no need to specify by default it takes => ansible atlanta -m command -a "/sbin/reboot"

    By default, Ansible perform tasks 5 managed nodes simultaneiously ,  that is nothing but fork count.
    
    If the managed nodes count more than fork count, it can increase the time it takes for Ansible to communicate with the hosts

   => So you can give the fork count , so it will perform at a time 10 managed nodes
      $ ansible atlanta -a "/sbin/reboot" -f 10

   => /usr/bin/ansible will default to running from your user account. To connect as a different user:
      $ ansible atlanta -a "/sbin/reboot" -f 10 -u username

   => Rebooting probably requires privilege escalation. You can connect to the server as username and run the command as the root user by using the become keyword:
      $ ansible atlanta -a "/sbin/reboot" -f 10 -u username --become [--ask-become-pass]
    If you add --ask-become-pass or -K, Ansible prompts you for the password to use for privilege escalation (sudo/su/pfexec/doas/etc).

``` 
<details>
<summary><b>Command Module example</b></summary>  
# ansible -a "ls /tmp" all
  
# ansible -a "cat /etc/centos-release" ungrouped

# ansible -m -a "df -h" london # it fails because if you mention -m then you should specify the module or you can use without -m

# ansible -m command -a "df -h"

# ansible -a "systemctl status sshd" all 

# ansible -a "systemctl stop sshd" all => not recommended -> use service module for any systemd or service realted activites , service will deal in smart way
  for example if you use systemctl start sshd in command module , if service started , still it tries to start, but service module it checks smart way, if started already, it won't do anything
</details>

  
<details>
<summary><b>Shell Module Examples</b></summary>

 Use shell module when:

Your command contains shell-specific features such as:
```
    Pipe	|
    Command codes `
    Redirect	> , >>
    Logical AND	&&
    Logical OR	`
    Wildcards (sometimes)	*.log
    Shell variables	$HOME, $TERM
    Command substitution	$(date)
    Any special characters
```
Examples:- 
```
       
  ansible all -m shell -a "ls /tmp | grep systemd"

  ansible all -m shell -a "echo hello > /tmp/test.txt"

  ansible all -m shell -a "yum install httpd -y && systemctl start httpd" # just for example

  ansible all -m shell -a "yum install httpd -y && systemctl start httpd"

  ansible all -m shell -a "echo $HOSTNAME" # it won't work , becuse echo $hostname runs locally and pass your hostaname to remote machine

  ansible all -m shell -a 'echo $HOSTNAME # it works

```
</details>

<details>
<summary><b> File Module Examples </b></summary>

=> BY using file module , we can create - directory,file, remove , change permissions as well.
### Below operation can be performed by a file module

```
  ansible webservers -m file -a "path=/opt/app state=touch" => creates an empty file

  ansible webservers -m file -a "path=/opt/app state=directory" => create a directory

  ansible webservers -m file -a "path=/opt/app state=absent" => deletes a directory

  ansible webservers -m file -a "path=/tmp/test.txt mode=0644" => change permissions

  ansible webservers -m file -a "path=/tmp/test.txt owner=ansible group=user" => change ownership

  ansible webservers -m file -a "src=/opt/app/config.yml dest=/etc/app.conf state=link" => create a symlink

  ansible webservers -m file -a "src=/opt/app/config.yml dest=/etc/app.conf state=hard" => create a hardlink

  ansible webservers -m file -a "path=/tmp/test.txt state=touch owner=appuser group=appgroup mode=0644" -b => All in one
  
  ansible all -m file -a "path=/india/ap/kadapa state=directory owner=ansible group=ansible mode=0755" -b => same like mkdir -p
```
</details>

<details>
<summary><b> Copy Module Examples </b></summary>

### By using copy module we can perfrom below

```
 # To create a file and write content
  ansible webservers -m copy -a "content='Hello World' dest=/tmp/test.txt owner=user1 group=user1 mode=0644" -b

 # Copy the file from control node to managed node
  ansible webservers -m copy -a "src=/tmp/app.conf dest=/etc/app.conf" -b 

 # Set the permissions while copying
  ansible webservers -m copy -a "src=/tmp/app.conf dest=/etc/app.conf mode=0644"

 # Set owner and group while copying
   ansible webservers -m copy -a "src=/tmp/app.conf dest=/etc/app.conf owner=root group=root" -b

 # Copy the entire directory along with it's content
   ansible webservers -m copy -a "src=/opt/app/ dest=/opt/"  

 # Create a backup before overwriting ==> it will create a backup file in remote machine at same location
   ansible webservers -m copy -a "src=/tmp/app.conf dest=/etc/app.conf backup=yes" -b

```            
</details>

<details>
<summary><b>Yum Module Examples</b></summary>

By using Yum module we can install, update, remove the packages

### To install without update
ansible -i [patotoinventory] -m yum -a "name=packagename state=present" -b (For root previlages)

```
  └─>>> ansible -i /user_data/file1/ansible/myhosts all -m yum -a "name=yakuake state=present" -b                                                          
  mumws6703 | CHANGED => {
      "ansible_facts": {
          "discovered_interpreter_python": "/usr/libexec/platform-python"
      },
      "changed": true,
      "msg": "",
      "rc": 0,
      "results": [
          "Installed: kf5-kglobalaccel-libs-5.96.0-1.el8.x86_64",
          "Installed: kf5-kiconthemes-5.96.0-1.el8.x86_64",
          "Installed: kf5-kinit-5.96.0-1.el8.x86_64",
          "Installed: kf5-kio-core-5.96.0-1.el8.x86_64",
          "Installed: kf5-kio-core-libs-5.96.0-1.el8.x86_64",
          "Installed: kf5-kio-doc-5.96.0-1.el8.noarch",
          "Installed: kf5-kwallet-5.96.0-1.el8.x86_64",
          "Installed: kf5-kwallet-libs-5.96.0-1.el8.x86_64",
          "Installed: kf5-kwayland-5.96.0-1.el8.x86_64",
          "Installed: yakuake-22.08.2-1.el8.x86_64",
          "Installed: kf5-kxmlgui-5.96.0-1.el8.x86_64",
          "Installed: kf5-kdoctools-5.96.0-1.el8.x86_64",
          "Installed: kf5-kglobalaccel-5.96.0-1.el8.x86_64"
      ]
  }
```
### To ensure a specific version of a package is installed:
 $ ansible -i /user_data/file1/ansible/myhosts all -m yum -a "name=yakuake-1.25 state=present" -b 

### To ensure a package is at the latest version:
 ansible -i /user_data/file1/ansible/myhosts all -m yum -a "name=yakuake-1.25 state=latest" -b

### To remove a package
```
  └─>>> ansible -i /user_data/file1/ansible/myhosts all -m yum -a "name=yakuake state=absent" -b                                                           [Wed, Jun 17 - 19:39:00][56s]
  mumws6703 | CHANGED => {
      "ansible_facts": {
          "discovered_interpreter_python": "/usr/libexec/platform-python"
      },
      "changed": true,
      "msg": "",
      "rc": 0,
      "results": [
          "Removed: yakuake-22.08.2-1.el8.x86_64"
      ]
  }
```
### Update All Packages
ansible all -m yum -a "name=* state=latest" -b
The above is equal to yum update -y  
###   

### Gathering Facts

```
  └─>>> ansible -i /user_data/file1/ansible/myhosts all -m setup                                                                                            [Wed, Jun 17 - 19:54:24][1s]
  mumws6703 | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "10.25.177.15",
            "192.168.122.1"
        ],
        "ansible_all_ipv6_addresses": [
            "fe80::250:56ff:fe99:7ea7"
        ],
        "ansible_apparmor": {
            "status": "disabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_bios_date": "11/12/2020",
        "ansible_bios_vendor": "Phoenix Technologies LTD",
        "ansible_bios_version": "6.00",
        "ansible_board_asset_tag": "NA",
        "ansible_board_name": "440BX Desktop Reference Platform",
        "ansible_board_serial": "NA",
        "ansible_board_vendor": "Intel Corporation",
        "ansible_board_version": "None",
        "ansible_chassis_asset_tag": "No Asset Tag",
        "ansible_chassis_serial": "NA",
        "ansible_chassis_vendor": "No Enclosure",
        "ansible_chassis_version": "N/A",
        "ansible_cmdline": {
            "BOOT_IMAGE": "/vmlinuz-4.18.0-553.115.1.el8_10.x86_64",
            "crashkernel": "256M",
            "nouveau.modeset": "0",
            "quiet": true,
            "rd.driver.blacklist": "cdrom,firewire_core,firewire_ohci,nouveau,sr_mod,usb_storage,uas",
            "rhgb": true,
            "ro": true,
            "root": "UUID=ed9b04d2-eda7-4eff-b6a5-5cb94b0d40b8",
            "selinux": "0"
        },
        "ansible_date_time": {
            "date": "2026-06-17",
            "day": "17",
            "epoch": "1781706286",
            "epoch_int": "1781706286",
            "hour": "19",
            "iso8601": "2026-06-17T14:24:46Z",
            "iso8601_basic": "20260617T195446038193",
            "iso8601_basic_short": "20260617T195446",
            "iso8601_micro": "2026-06-17T14:24:46.038193Z",
            "minute": "54",
            "month": "06",
            "second": "46",
            "time": "19:54:46",
            "tz": "IST",
            "tz_dst": "IST",
            "tz_offset": "+0530",
            "weekday": "Wednesday",
            "weekday_number": "3",
            "weeknumber": "24",
            "year": "2026"
        },

```
</details>

<details>
<summary><b> Service Module Examples </b></summary>

### By using service module we can do "start stop restart & reload Operations "

### To start a service
```
httpd is not started and in disabled state
===============================================================
 Scenario 1 :- when service is in stopped state the above command - started successfully but, still disabled
 httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; preset: d>
     Active: inactive (dead)
       Docs: man:httpd.service(8)

[root@server ~]# ansible all -m service -a "name=httpd state=started"
192.168.240.129 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": true,  =====> when service was stopped changed = true beacuse it started
    "name": "httpd",
    "state": "started",
    "status": {

  Scenario 2 :- when service is already started the above command - started successfully & changed = false
  192.168.240.129 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "name": "httpd",
    "state": "started",
    "status": {

```
### To enable and start
[root@server ~]# ansible all -m service -a "name=httpd state=started enabled=yes"

### To stop
[root@server ~]# ansible all -m service -a "name=httpd state=stopped"

### To restart
[root@server ~]# ansible all -m service -a "name=httpd state=restarted"

### To reload
[root@server ~]# ansible all -m service -a "name=httpd state=reloaded"

# Key thing to remember -> 
#   current = desired state ===> true
#   current != desired state ===> false

</details>

<details> <summary><b> User Module Examples </b></summary>

# Used to manage users (create, modify, delete, lock)

```
  User does not exist
  ===============================================================
  Scenario 1 :- user created successfully

  [root@server ~]# ansible all -b -m user -a "name=john"

  192.168.240.129 | CHANGED => {
      "changed": true,
      "name": "john",
      "state": "present"
  }

  Scenario 2 :- user already exists

  192.168.240.129 | SUCCESS => {
      "changed": false,
      "name": "john"
  }

```
# Create user with shell
 ansible all -b -m user -a "name=john shell=/bin/bash"

# Add user to group
 ansible all -b -m user -a "name=john groups=wheel"

# Create a user with home directory
 ansible all -b -m user -a "name=john home=/home/john"

# Delete user
 ansible all -b -m user -a "name=john state=absent"

# Delete with home
 ansible all -b -m user -a "name=john state=absent remove=yes"

# Locked user 
 ansible all -b -m user -a "name=john password_lock=yes"

</details>

<details> <summary><b> Lineinfile Module Examples </b></summary>
Used to add, modify, or remove specific lines in a file
Add a line
File: /tmp/test.txt
===============================================================
Scenario 1 :- line added

[root@server ~]# ansible all -b -m lineinfile -a "path=/tmp/test.txt line='Hello World'"

192.168.240.129 | CHANGED => {
    "changed": true
}

Scenario 2 :- line already exists

192.168.240.129 | SUCCESS => {
    "changed": false
}
Modify existing line
ansible all -b -m lineinfile -a "path=/etc/ssh/sshd_config regexp='^PermitRootLogin' line='PermitRootLogin no'"
Add entry in hosts file
ansible all -b -m lineinfile -a "path=/etc/hosts line='192.168.1.10 appserver'"
Remove a line
ansible all -b -m lineinfile -a "path=/etc/hosts regexp='appserver' state=absent"
Key thing to remember
Line exists & correct => changed: false
Line missing or different => changed: true
</details>
<details> <summary><b> Systemd Module Examples </b></summary>
Used to manage system services in modern Linux systems
Start service
ansible all -b -m systemd -a "name=httpd state=started"
Stop service
ansible all -b -m systemd -a "name=httpd state=stopped"
Restart service
ansible all -b -m systemd -a "name=httpd state=restarted"
Enable service at boot
ansible all -b -m systemd -a "name=httpd enabled=yes"
Disable service
ansible all -b -m systemd -a "name=httpd enabled=no"
Reload systemd daemon
ansible all -b -m systemd -a "daemon_reload=yes"
Key thing to remember
Systemd ensures desired state of service
Same state => changed: false
Different state => changed: true
</details>

<details>
<summary><b> Service vs Systemd Module Differences </b></summary>

---

## What is common?

Both modules are used to:
- Start services
- Stop services
- Restart services
- Enable services at boot
- Check service state

---

## Key Difference

### 1. Service Module

- Older, generic module
- Works with multiple init systems
- Can work on:
  - SysVinit
  - Upstart
  - Systemd (internally maps to systemctl in modern OS)

```text id="srv01"
Service module = compatibility layer (generic)
2. Systemd Module
Modern module
Works only with systemd-based systems
Directly interacts with systemd (systemctl)
Systemd module = native systemd controller
Important Differences Table
Feature	service module	systemd module
Type	Generic module	Modern module
Works with	SysV, Upstart, Systemd	Only Systemd
Speed	Slightly slower	Faster
Control level	Abstract layer	Direct systemctl control
Recommended	Legacy systems	Modern Linux (RHEL 7+, Ubuntu 16+)
Usage today	Less preferred	Most used
Real-world usage
Service module usage (legacy / compatibility)
ansible all -b -m service -a "name=httpd state=started"
Systemd module usage (modern preferred way)
ansible all -b -m systemd -a "name=httpd state=started"
Interview Key Points
1. systemd module is preferred over service module in modern Linux
2. service module is backward compatible wrapper
3. systemd gives direct control via systemctl
4. Both achieve same result, but systemd is more precise and reliable
Final Summary
Use service module → when you want compatibility across old systems

Use systemd module → when working on modern Linux (real DevOps environments)

</details>

