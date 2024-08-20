# Problem Statement

---
Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:

- a. On jump host create an Ansible playbook _/home/thor/ansible/playbook.yml_ and configure it to install _vsftpd_ on all app servers.

- b. After installation make sure to _start_ and _enable_ vsftpd service on all app servers.

- c. The inventory _/home/thor/ansible/inventory_ is already there on jump host.

- d. Make sure user _thor_ should be able to run the playbook on jump host.

_Note_: Validation will try to run playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure playbook works this way, without passing any extra arguments.

## Solution

1. **Navigate to the Ansible directory:**

   ```sh
   cd /home/thor/ansible
   ```

2. **Create the Ansible playbook file:**
   Use a text editor to create and edit the `playbook.yml` file.

   ```sh
   vi playbook.yml
   ```

3. **Add the following YAML configuration to the `playbook.yml` file:**

   ```yaml
   ---
   - hosts: all
     become: yes
     tasks:
       - name: Install vsftpd on all app servers
         yum:
           name: vsftpd
           state: present
       - name: Start and enable the vsftpd service
         service:
           name: vsftpd
           state: started
           enabled: true
   ```

5. **Ensure the user `thor` can run the playbook:**
   You need to check that the `thor` user has the appropriate permissions to execute Ansible commands and run playbooks. Ensure the user has SSH access to the managed nodes and the necessary Ansible configuration.

6. **Test the playbook:**
   Execute the following command to run the playbook using the specified inventory file:

   ```sh
   ansible-playbook -i inventory playbook.yml
   ```

### Expected Output

When you run the playbook, you should see output similar to the following, indicating that the playbook has successfully installed `vsftpd`, started, and enabled the service on all app servers:

```
PLAY [all] ******************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp01]
ok: [stapp03]
ok: [stapp02]

TASK [Install vsftpd on all app servers] ************************************************
ok: [stapp03]
ok: [stapp01]
ok: [stapp02]

TASK [Start and enable vsftpd service] **************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

PLAY RECAP ******************************************************************************
stapp01                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
stapp02                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
stapp03                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

This output indicates that all tasks were executed successfully and no changes were required because the `vsftpd` service was already in the desired state.
