# Ansible Hands ON

Ansible hands-on practice and documentation

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
    * hosts.yml
    * group_vars/
    * host_vars/

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

## Callback plugins

Callback plugins in Ansible are used to customize the output and behavior of playbook execution. They allow you to hook into different stages of a playbook run and perform actions such as:

1. Logging results to external systems (e.g., log files, databases)
2. Sending notifications (e.g., Slack, email, webhook alerts)
3. Formatting output (e.g., human-readable, JSON, minimal)
4. Triggering post-playbook actions like audits or reports

## Types of Inventory

1. Static Inventory \
Defined manually in an INI or YAML file.
Hosts and groups are explicitly listed.
Simple and ideal for small or stable environments.

2. Dynamic Inventory \
Generated automatically using scripts or plugins.
Pulls host data from external sources like AWS, Azure, GCP, or CMDBs.
Ideal for cloud-native or large-scale environments with frequent changes.

## Ad-hoc commands
In Ansible, an ad-hoc command is a fast, one liner task that you perform directly from the command line. This command helps for quick fixes or checks on remote systems.
`ansible group1  -m shell -a 'df -h'`

## Ansible Tower

Ansible Tower is a web-based interface developed by Red Hat that enhances the usability and scalability of Ansible automation. It provides a centralized platform for managing playbooks, inventories, credentials, and workflows through a visual dashboard. Key features include role-based access control, which ensures secure delegation of tasks among users; job scheduling, allowing playbooks to run automatically at specified times; and real-time monitoring and logging, which helps track execution and troubleshoot issues. Tower also supports workflow orchestration, enabling complex automation sequences, and integrates with external systems like Git, LDAP, and cloud providers for seamless enterprise use.

## To improve the performance of a slow playbook

* Enable SSH Pipelining – Reduces overhead by reusing SSH connections, speeding up task execution.
* Increase Forks – Boosts parallelism by allowing Ansible to manage more hosts simultaneously (default is 5).
* Configure Fact Caching – Stores host facts to avoid re-gathering them on every run, saving time across large inventories.

## To debug a failing Ansible task

* Increase verbosity using -vvv to get detailed output.
* Isolate the issue with --limit and --start-at-task.
* Use debug and register to inspect variables and task output.
* Run in check mode with --check --diff to preview changes.

## Delegate tasks In Ansible

Delegating tasks in Ansible is a powerful feature that allows you to execute specific tasks on a host different from the one currently targeted by the play. This is done using the delegate_to keyword within a task definition. It’s especially useful in scenarios where centralized operations are required-such as updating a load balancer, managing a shared database, or collecting logs from multiple nodes.

example:
```
- name: Update DNS records from webserver play
  hosts: webservers
  tasks:
    - name: Update DNS on central server
      delegate_to: dns-master
      run_once: true
      ansible.builtin.command: /usr/bin/update-dns
```

## What Is Ansible Registry?

The term Ansible registry is sometimes informally used to describe mechanisms for persistently storing and sharing variables across tasks or plays in a playbook.

1. Registered variables: Using the register keyword to capture task output and reuse it later in the playbook.
2. Set_fact module: To define variables dynamically during runtime and make them available to subsequent tasks.
3. Host and group variables: Stored in inventory files or variable directories to persist values across plays.
4. Fact caching: Enables variables (facts) to be stored between playbook runs using backends like Redis or JSON.

## Ansible Galaxy

Ansible Galaxy is a central repository and community hub for sharing, finding, and reusing Ansible content. Think of it like Docker Hub for container images.

```
# Search for roles
ansible-galaxy search "nginx"

# Install a role
ansible-galaxy role install geerlingguy.nginx

# Install a collection
ansible-galaxy collection install community.general

# List installed roles
ansible-galaxy role list

# Initialize a new role structure
ansible-galaxy role init my_new_role
```

## Setting Dynamic Variables

We use `set_fact` , Sets variables dynamically during playbook execution.

example::
```
- hosts: all
  tasks:
    - name: Set a simple fact
      set_fact:
        greeting: "Hello, World!"
    
    - name: Use the fact
      debug:
        msg: "{{ greeting }}"
```








