# Ansible Hands ON
Ansible hands-on practice and documentation

##### Ref: https://www.geeksforgeeks.org/devops/ansible-interview-questions/

## Why Ansible ?
Ansible is an open-source automation platform used to configure systems, deploy applications, and orchestrate IT workflows across servers, containers, network gear, and cloud services.

### Key Components
1. Control Node: where playbooks are executed
2. Managed Nodes: target systems

##
* It communicates using standard protocols SSH for Linux/Unix and WinRM for Windows without requiring any agent installation.

* Ansible doesn’t require software agents on managed nodes; it uses SSH or WinRM for communication. In contrast, Puppet and Chef rely on agents that poll a central server for updates a pull-based model.

## What are idempotent modules ?

Module is idempotent if it runs at first , the result have no different from when it runs repeatedly.

* but Not all Ansibles modules are idempotent! like `shell` and `command`. We should used `when` or `changed when` for them to be idempotent.

## Structure of Ansible Project

1. Inventory:
    * hosts.ym
    * group_vars/
    *   host_vars/

* extra_vars > host_vars > group_vars > inventory vars > role defaults

2. role:
    * tasks: tasks to run for playbook
    * handlers
    * defaults: sets low-priority, user-overridable variables
    * vars: holds high-priority internal variables
    * meta: includes role metadata and dependencies.
    * templates
    * files

## What are handlers ?
In Ansible, a handler is a special task triggered only when notified by another task. It’s typically used for actions like restarting services after configuration changes. Handlers solve the problem of unnecessary repetition by ensuring tasks run only when needed.

* Only run if notified
* Run once (in one play) even if notified more
* Run at the end of play

* playbook -> some plays -> some tasks




