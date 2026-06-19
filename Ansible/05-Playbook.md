### Playbook is nothing but collection of tasks

Till the time we have learn executed different modules with ad hoc commands
But in playbook - we are going to keep all the tasks and going run on managed nodes
For example if We mount mount user_data on managed nodes
So the task we manually going to perform like below
```
#### Procedure for mounting user_data
Steps:-
  step 1 - checking disk - lsblk
  step 2 - create the partition fdisk /dev/sdb
  step 3 - making the filesystem mkfs.xfs /dev/sdb1
  step 4 - create the /user_data directory with 777 permissions
  step 5 - giving entry in fstab
  step 6 - mount the /user_data
```
if you want do through ad hoc command you have to run each step seperatly at mutliple times, But by using playbook we can complete the task in one go

<details><summary><b> Playbook for Above Task</b></summary>

## playbook for above task
```yaml
    you need to create a playboon with .yaml or yml extension
    --- # start of the playbook/ yaml document
    - name: Your playbook name or what your play book do or we can say Title of the playbook
      hosts: In which managed nodes you are going to perform
      gather_facts: no # By defualt ansible will gather the facts of managed nodes to disbale we can use no
      become: yes   # To perform as root user

    tasks # If we can take above example tasks is nothing but "steps"
    - name: Checking the disk    # task1 or step1
      command:  lsblk # the module name is command and the command is lsblk
      register: lsblk_output  # by default it won't show the output hence we are storing output in lsblk_output

    - name: Show the output 
      debug:  
        var: lsblk_output.std_output  # Since ansible prints all the details what command run , rc status , we don't want all hence using .std_output

    - name: Create the partition  # task 2 or step2
      partd: 
        device: /dev/sdb
        number: 1
        state: present
        part_start: 1MiB
        part_end: 100%

    - name: making directory # task 3 or step3
      file:
        path: /user_data
        state: directory
        mode: 0777

    - name: Creat Xfs filesystem #taks 4 or step4
      filesystem: 
        fstype: xfs
        dev: /dev/sdb1

    - name: add entry in fstab # task 5 or step5
      mount:
        path: /user_data
        src: /dev/sdb1
        fstype: xfs
        opts: rw,defaults
        state: mounted      # it will mount, we no need to run/write mount /user_data

```
## This is a simple playbook 
## Allignment is very important
### Space must be given after - and : 
</details>

## How to run a playbook

 ### Ansible-playbook -i [IF you have custom inventory file] playbook.yaml 

 ### Ansible-playbook -i [IF you have custom inventory file] playbook.yaml --syntax-check   # to check the syntax

 ### Ansible-playbook -i [IF you have custom inventory file] playbook.yaml --list-hosts    # in which it going to perform

 ### Ansible-playbook -i [IF you have custom inventory file] playbook.yaml --list-tasks    # list of tasks

 ### Ansible-playbook -i [IF you have custom inventory file] playbook.yaml --list-tags     # show tags

 ### Ansible-playbook -i [IF you have custom inventory file] playbook.yaml --check         # dry run

 ### ansible-playbook -i inventory.ini playbook.yaml -vvv # show detail information while execution to find the error incase Important

 ### ansible-playbook -i inventory.ini playbook.yaml --check --diff  # Dry run and what going to change will show
