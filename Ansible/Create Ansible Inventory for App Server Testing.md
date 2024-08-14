## Problem Statement

The Nautilus DevOps team is testing Ansible playbooks on various servers within their stack. They've placed some playbooks under _/home/thor/playbook/_ directory on the _jump_ host and now intend to test them on _app server 3_ in _Stratos DC_. However, an inventory file needs creation for Ansible to connect to the respective app. Here are the requirements:

- a. Create an _ini_ type Ansible inventory file _/home/thor/playbook/inventory_on _jump host_.

- b. Include _App Server 3_ in this inventory along with necessary variables for proper functionality.

- c. Ensure the inventory hostname corresponds to the _server_ name as per the wiki, for example _stapp01_ for _app server 1_ in Stratos DC.

Note: Validation will execute the playbook using the command _ansible-playbook -i inventory playbook.yml._ Ensure the playbook functions properly without any extra arguments.

## Solution

### 1. Create the Inventory File

```bash
# Use a text editor to create or edit the inventory file
vi /home/thor/playbook/inventory
```

Add the following content to the file:

```ini
# /home/thor/playbook/inventory
[app_servers]
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n
```

### 2. Verify the Playbook

The provided `playbook.yml` is used to install and start the `httpd` service. Ensure that the playbook syntax and indentation are correct.

```yaml
# /home/thor/playbook/playbook.yml
---
- hosts: all
  become: yes
  become_user: root
  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: installed

    - name: Start service httpd
      service:
        name: httpd
        state: started
```

### 3. Test the Playbook Execution

Run the playbook to ensure everything is working as expected:

```bash
ansible-playbook -i inventory playbook.yml
```

### Final Validation

Make sure that after executing the playbook, the following output is produced:

```plaintext
PLAY [all] ******************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp03]

TASK [Install httpd package] ************************************************************
changed: [stapp03]

TASK [Start service httpd] **************************************************************
changed: [stapp03]

PLAY RECAP ******************************************************************************
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
