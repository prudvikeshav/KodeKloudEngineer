# Problem Statement

---
Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:

- a. On jump host create an Ansible playbook _/home/thor/ansible/playbook.yml_ and configure it to install _vsftpd_ on all app servers.

- b. After installation make sure to _start_ and _enable_ vsftpd service on all app servers.

- c. The inventory _/home/thor/ansible/inventory_ is already there on jump host.

- d. Make sure user _thor_ should be able to run the playbook on jump host.

_Note_: Validation will try to run playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure playbook works this way, without passing any extra arguments.
