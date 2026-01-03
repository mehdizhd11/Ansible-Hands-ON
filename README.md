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

2. role(organize automation content):
    * tasks: tasks to run for playbook
    * handlers
    * defaults: sets low-priority, user-overridable variables
    * vars: holds high-priority internal variables
    * meta: includes role metadata and dependencies.
    * templates
    * files

3. playbook

## What are handlers ?
In Ansible, a handler is a special task triggered only when notified by another task. It’s typically used for actions like restarting services after configuration changes. Handlers solve the problem of unnecessary repetition by ensuring tasks run only when needed.

* Only run if notified
* Run once (in one play) even if notified more
* Run at the end of play

* playbook -> some plays -> some tasks

## Example of playbook
* Install a Package:
```
- name: Install Package Playbook
  hosts: your_server_group
  become: true  # This allows running tasks with elevated privileges (sudo)
  tasks:
  - Install the desired package
     apt:
        name: your_package_name 
        state: present  # You can use 'latest' to ensure the latest version
```

`ansible-playbook -i /path/to/inventory/file myplaybook.yml`

* Check The Status Of a Service
```
- name: Check Service Status
  hosts: your_server_group
  become: true  # This allows running tasks with elevated privileges (sudo)

  tasks:
    - name: Check status of the 'your_service_name' service
       service_facts:
         name: your_service_name  # Replace with the actual service name

    - name: Display the service status
       debug:
         var: ansible_facts.services['your_service_name'].state
```

## What is jump host ?
A jump host or proxy host an intermediary server that is used to access other servers in a network that are not directly reachable from the ansible control machine. We have 2 solution:

1. Edit SSH Config

2. Use jump Host in playbook:
```
- name: Your Playbook
  hosts: your_target_hosts
  vars:
    ansible_ssh_common_args: '-o ProxyJump=jump_host'
```

## What is Ansible Vault ?
Ansible Vault is a feature that allows users to encrypt values and data structures within Ansible projects.
* Creating New Encrypted Files
   `ansible-vault create vault.yml`
  
* Encrypting Existing Files
  `ansible-vault encrypt encrypt_me.txt`

* Viewing Encrypted Files
  `ansible-vault view vault.yml`

* Editing Encrypted Files
  `ansible-vault edit vault.yml`

example:
`ansible-playbook -i inventory/inv.ini site.yml --ask-vault-pass`






