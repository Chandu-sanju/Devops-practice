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


