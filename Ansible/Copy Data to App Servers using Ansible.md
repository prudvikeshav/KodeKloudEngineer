## Problem statement

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:

a. Create an inventory file _/home/thor/ansible/inventory_ on _jump_host_ and add all application servers as managed nodes.

b. Create a playbook _/home/thor/ansible/playbook.yml_ on the _jump host_ to copy the _/usr/src/itadmin/index.html_ file to all application servers, placing it at _/opt/itadmin_.

Note: Validation will run the playbook using the command _ansible-playbook -i inventory playbook.yml_. Ensure the playbook functions properly without any extra arguments.
