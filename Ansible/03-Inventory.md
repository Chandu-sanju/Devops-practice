# What is inventory file in Ansible
```
  =>>> Inventory file contains list or group of your managed nodes hostname or Ip address

  =>>> Default inventory file is located at /hosts/ansible/hosts, You no need to specify explicitly

  =>>> Sometime based on our requirement, When you are handling mutiple projects, You can use inventory file

  =>>> While running you have to specify with -i flag and Path respectively

  =>>> Example: ansible -i /tmp/Wipro/USA -m ping all
```  
 # How to add hosts or machine in inventory file
====> We can write in INI format

====> or we can write in Yaml format

=====> both can be acceptable

<details>
<summary><b><font color="red"> INI Format </font></b></summary>

```
 ## hostnames or ip address we can specify
   192.168.33.21
   192.156.78.22
   jira1@dneg.com
   mtlws9011
   db1@dneg.com
  
 ## Combining machines into a group for easy of administration

    [Database]
    192.168.33.21
    192.156.78.22

    [Jiraservers]
    jira1@dneg.com
    mtlws9011
    db1@dneg.com

    ##### Not valid ############################

    [Data  base] # nospace between group names
    192.168.33.21
    192.156.78.22

    [ Jiraservers ] # No space between []
    jira1@dneg.com
    mtlws9011
    db1@dneg.com

    #### Valid ##################################
    
    [Data_base]
    192.168.33.21
    192.156.78.22

    [Jira-servers]
    jira1@dneg.com
    mtlws9011
    db1@dneg.com

``` 
## Default Groups 
> [!IMPORANT]
>  Ansible has 2 defaults groups ungrouped and all
> By default each managed node part of alteast two groups
> a managed node can be part of mulitple groups
> if not under any group => then it is part of ungrouped + all
> if under any group => that group + all
```
 #MY inventory file is below
 sydws5    ===> ungrouped +all
 [london]
 lonws153 ====> all + london
 lonws2255 ===> all + london
 lonws2396 ===> all + london

 [montreal]
 mtlws9011  ====> all + montreal
 mtlws9011  ====> all + montreal

 [zero]
 sydws5
 lonws153 ## not it is part of mutliple groups

```
## NESTED GROUPS
====> Group inside groups
```
    [london]
    lonws153
    lonws2255

    [montreal]
    mtlws9012
    mtlws9013

    [canada:children] # Canada is parent montreal is child
    montreal

    [world:children] World is parent and london and canada is child
    london
    canada
    world
    ├── london
    │   ├── lonws153
    │   └── lonws2255
    └── canada
        └── montreal
            ├── mtlws9012
            └── mtlws9013
```



</details>

<details>
<summary><b><font color="green">Before Going to dive into deept about inventory file let's learn yaml syntax </font></b></summary>

 ## Key-value Pair
 You can secify as blelow
 ```
    key: value
    user1: chandu
    user2: aanchal
    user3: sangam
    user4: kajal
```    
> [!NOTE]
> After key immediate colon and a space is required
## List
List of items or Mutliple items
```
    Employees:
           - Chandu
           - Aanchal
           - Sangam
           - Mounika
```
> [!NOTE]
> All the list should be come in one order, if you give multiple space in ansible works , but yaml syntax is allow one or two spaces.
> you can get this error - expects 2 spaces (or 1 indentation level).
> if you want to test- install yammlint - pip install yamllint - then create a file and execute yamllint filename
## Dictionary
Dictionary means one object/item or one person set of details
```
    `└─>>> bat dic.yml
    ───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        │ File: dic.yml
    ───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    1   │ ---
    2   │ # Employee Details
    3   │ name: Chandra
    4   │ Email: vcse@dneg.com
    5   │ WorkdayID: 018791
    6   │ Projects:
    7   │  - Tech support
    8   │  - SLHR
    9   │  - TRM
    10   │  - PTSUP
    ───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    ┌──(vcse@chnws5245)-[~/ansible]
    └─>>> yamllint dic.yml

```
> [!NOTE]
> No output means no syntax error
## List of Dictionaries
Where each item or object or personal as dictionary # mutiple employe details
```
   `└─>>> bat dic.yml
    ───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
        │ File: dic.yml
    ───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    1   │ ---
    2   │ #First Employee Details
    3   │ name: Chandra
    4   │ Email: vcse@dneg.com
    5   │ WorkdayID: 018791
    6   │ Projects:
    7   │  - Tech support
    8   │  - SLHR
    9   │  - TRM
    10  │  - PTSUP
    11  │ #Second Employee Details
    12  │ name: Chandra
    13  │ Email: vcse@dneg.com
    14  │ WorkdayID: 018791
    15  │ Projects:
    16  │  - Tech support
    17  │  - SLHR
    18  │  - TRM
    19  │  - PTSUP


    ───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    ┌──(vcse@chnws5245)-[~/ansible]
    └─>>> yamllint dic.yml

```
## Variables
==> Varibles nothing but key-value pairs
===> all the variable are key-value pairs But all the key-vaules are not varaibles
``
vars:
  package_name: nginx
  version1: 2026.01.02
  version2: 2027.01.03

tasks:
  - name: Install package
    yum:
      name: "{{ package_name }}"
      version: "{{ version1 }} 

tasks:
  - name: Install package
    yum:
      name: "{{ package_name }}"
      version: "{{ version2 }} 
```
## Mutliline strings
For suppose you want to specify any mutline strings or write someting in a file
if normal string  you can ===>
                    message: How are you ?
Multiline ===>
              message: |
                  Hi How are you
                  what are you doing
                  I hope everything fine
```
</details>