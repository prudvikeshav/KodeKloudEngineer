# Problem Statement

---
The Nautilus DevOps team is trying to setup a simple Apache web server on all app servers in Stratos DC using Ansible. They also want to create a sample html page for now with some app specific data on it. Below you can find more details about the task.

You will find a valid inventory file _/home/thor/playbooks/inventory_ on _jump host_ (which we are using as an Ansible controller).

- Create a playbook _index.yml_ under _/home/thor/playbooks_ directory on _jump host_. Using _blockinfile_ Ansible module create a file _facts.txt_ under _/root_ directory on all app servers and add the following given block in it. You will need to enable facts gathering for this task.

      _Ansible managed node architecture is <architecture>_

(You can obtain the system architecture from Ansible's gathered facts by using the correct Ansible variable while taking into account Jinja2 syntax)

- Install _httpd_ server on all apps. After that make a copy of _facts.txt_ file as _index.html_ under_/var/www/html_ directory. Make sure to start _httpd_ service after that.

Note: Do not create a separate role for this task, just add all of the changes in _index.yml_ playbook.
