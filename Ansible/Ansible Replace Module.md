# Problem Statement

---
There is some data on all app servers in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Write a playbook.yml under _/home/thor/ansible_ on jump host, an inventory is already present under _/home/thor/ansible_ directory on Jump host itself. Perform below given tasks using this playbook:

We have a file _/opt/devops/blog.txt_ on app server 1. Using Ansible _replace_ module replace string _xFusionCorp_ to _Nautilus_ in that file.

We have a file _/opt/devops/story.txt_ on app server 2. Using Ansible _replace_ module replace the string _Nautilus_ to _KodeKloud_ in that file.

We have a file _/opt/devops/media.txt_ on app server 3. Using Ansible _replace_ module replace string _KodeKloud_ to _xFusionCorp Industries_ in that file.

Note: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure the playbook works this way without passing any extra arguments.
