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
