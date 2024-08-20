# Problem  Statement

---
There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

- Create a playbook named _playbook.yml_ under _/home/thor/ansible_ directory on jump host, an _inventory_ file is already present under _/home/thor/ansible_ directory on Jump Server itself.

- Create an empty file blog.txt under _/opt/sysops/_ directory on app server 1. Set some _acl_ properties for this file. Using _acl_ provide _read '(r)'_ permissions to group _tony_ (i.e _entity_ is _tony_ and _etype_ is group).

- Create an empty file story.txt under _/opt/sysops/_ directory on app server 2. Set some _acl_ properties for this file. Using _acl_ provide _read + write '(rw)_' permissions to user _steve_ (i.e _entity_ is _steve_ and _etype_ is user).

- Create an empty file _media.txt_ under _/opt/sysops/_ on app server 3. Set some _acl_ properties for this file. Using _acl_ provide _read + write '(rw)_' permissions to group _banner_ (i.e _entity_ is _banner_ and _etype_ is group).

_Note_: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure the playbook works this way, without passing any extra arguments.
