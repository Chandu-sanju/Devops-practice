# Ansible Tags - Complete Notes

## What are Tags?

Tags are labels assigned to tasks, blocks, plays, or roles that allow selective execution of specific parts of a playbook.

Instead of running the entire playbook, you can run only the tagged sections.

---

# Why Use Tags?

### Faster Execution

Run only required tasks.

```bash
ansible-playbook site.yml --tags nginx
```

### Easier Troubleshooting

Test a specific section without running everything.

### Partial Deployments

Deploy only applications, databases, or configurations.

### CI/CD Integration

Run deployment-specific tasks in pipelines.

---

# Basic Syntax

## Single Tag

```yaml
- name: Install Apache
  package:
    name: httpd
    state: present
  tags:
    - apache
```

Run:

```bash
ansible-playbook site.yml --tags apache
```

---

## Multiple Tags

```yaml
- name: Install Apache
  package:
    name: httpd
    state: present
  tags:
    - apache
    - web
    - install
```

Run:

```bash
ansible-playbook site.yml --tags web
```

or

```bash
ansible-playbook site.yml --tags install
```

---

# Where Can Tags Be Applied?

## Task Level

```yaml
- name: Install Apache
  package:
    name: httpd
    state: present
  tags:
    - apache
```

---

## Block Level

```yaml
tasks:

  - block:

      - name: Install Apache
        package:
          name: httpd
          state: present

      - name: Start Apache
        service:
          name: httpd
          state: started

    tags:
      - apache
```

All tasks inside the block inherit the tag.

---

## Play Level

```yaml
- hosts: webservers

  tags:
    - web

  tasks:

    - name: Install Apache
      package:
        name: httpd
        state: present
```

Run:

```bash
ansible-playbook site.yml --tags web
```

---

## Role Level

```yaml
- hosts: all

  roles:

    - role: nginx
      tags:
        - web

    - role: mysql
      tags:
        - db
```

Run:

```bash
ansible-playbook site.yml --tags web
```

Only nginx role executes.

---

# Running Tagged Tasks

## Run Single Tag

```bash
ansible-playbook site.yml --tags apache
```

---

## Run Multiple Tags

```bash
ansible-playbook site.yml --tags "apache,db"
```

Runs tasks matching either tag.

---

# Skip Tags

Skip specific tagged tasks.

```bash
ansible-playbook site.yml --skip-tags security
```

Example:

```yaml
- name: Security Update
  package:
    name: '*'
    state: latest
  tags:
    - security
```

---

# List Available Tags

```bash
ansible-playbook site.yml --list-tags
```

Example Output:

```text
playbook: site.yml

play #1

TASK TAGS:
[apache, db, security]
```

---

# List Tasks

```bash
ansible-playbook site.yml --list-tasks
```

Shows all tasks that would run.

---

# Special Tags

## always

Runs regardless of tag selection.

```yaml
- name: Gather Facts
  setup:
  tags:
    - always
```

Even when running:

```bash
ansible-playbook site.yml --tags apache
```

The task executes.

### Common Uses

* Fact gathering
* Validation
* Pre-checks

---

## never

Task does not run unless explicitly requested.

```yaml
- name: Reset Database
  command: reset_db.sh
  tags:
    - never
    - resetdb
```

Normal run:

```bash
ansible-playbook site.yml
```

Task skipped.

Explicit run:

```bash
ansible-playbook site.yml --tags resetdb
```

---

# Tag Inheritance

Tags applied at:

* Play level
* Block level
* Role level

are inherited by child tasks.

Example:

```yaml
- hosts: webservers

  tags:
    - web

  tasks:

    - name: Install Apache
      package:
        name: httpd
        state: present
```

The task automatically inherits the `web` tag.

---

# Tag Positioning

Tags are task-level keywords.

Correct:

```yaml
- name: Install Apache
  package:
    name: httpd
    state: present
  tags:
    - apache
```

Also Correct:

```yaml
- name: Install Apache
  tags:
    - apache
  package:
    name: httpd
    state: present
```

Both work.

---

# Incorrect Usage

Wrong:

```yaml
- name: Install Apache
  package:
    name: httpd
    state: present
    tags:
      - apache
```

Reason:

`tags` is not a module parameter.

It must be at the same level as the module.

---

Wrong:

```yaml
- name: Install Apache
  package:
    name: httpd
    state: present
  - tags:
      - apache
```

Reason:

The `-` starts a new list item and breaks YAML structure.

---

# Common Production Example

```yaml
- hosts: all

  roles:

    - role: common
      tags:
        - common

    - role: nginx
      tags:
        - web

    - role: mysql
      tags:
        - db

    - role: deploy
      tags:
        - deploy
```

Commands:

Run only common setup:

```bash
ansible-playbook site.yml --tags common
```

Run deployment:

```bash
ansible-playbook site.yml --tags deploy
```

Run web and database:

```bash
ansible-playbook site.yml --tags "web,db"
```

---

# Interview Questions

## What are Tags?

Tags are labels that allow selective execution of tasks, blocks, plays, or roles.

---

## Why Use Tags?

* Faster execution
* Easier debugging
* Partial deployments
* Better playbook management
* CI/CD automation

---

## How Do You Run Only Tagged Tasks?

```bash
ansible-playbook site.yml --tags web
```

---

## How Do You Skip Tagged Tasks?

```bash
ansible-playbook site.yml --skip-tags security
```

---

## How Do You View Available Tags?

```bash
ansible-playbook site.yml --list-tags
```

---

## Difference Between always and never?

| Tag    | Purpose                             |
| ------ | ----------------------------------- |
| always | Runs every time                     |
| never  | Runs only when explicitly requested |

---

## Can a Task Have Multiple Tags?

Yes.

```yaml
tags:
  - install
  - web
  - apache
```

---

## Can Tags Be Applied to Roles?

Yes.

```yaml
roles:
  - role: nginx
    tags:
      - web
```

---

## What is Tag Inheritance?

Tags applied to plays, blocks, and roles are automatically inherited by child tasks.

---

# Quick Revision

```text
Tags = Labels for selective execution.

Apply tags on:
✓ Tasks
✓ Blocks
✓ Plays
✓ Roles

Commands:

--tags web
Run only web tasks

--tags "web,db"
Run web and db tasks

--skip-tags security
Skip security tasks

--list-tags
Show available tags

--list-tasks
Show tasks

Special Tags:

always
Runs every time

never
Runs only when explicitly requested

Benefits:

✓ Faster execution
✓ Easier debugging
✓ Partial deployments
✓ CI/CD friendly
✓ Better playbook management
```
