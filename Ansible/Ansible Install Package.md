# Problem Statement

---
The Nautilus Application development team wanted to test some applications on app servers in Stratos Datacenter. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers. Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:

- Create an inventory file _/home/thor/playbook/inventory_ on _jump host_ and add _all app servers_ in it.

- Create an Ansible playbook _/home/thor/playbook/playbook.yml_ to install _httpd_ package on all  _app servers_ using Ansible _yum_ module.

- Make sure user thor should be able to run the playbook on _jump host_.

## Solution

---

1. **Navigate to the playbook directory:**

   ```bash
   cd /home/thor/playbook/
   ```

2. **Create and edit the inventory file:**

   Open the inventory file with your preferred text editor:

   ```bash
   vi inventory
   ```

   Add the following content to define the app servers:

   ```ini
   [app_servers]
   stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
   stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
   stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n
   ```

3. **Verify connectivity to the app servers:**

   Run a basic Ansible command to ensure you can communicate with the app servers:

   ```bash
   ansible app_servers -m ping -i inventory
   ```

   You should see a response indicating successful communication:

   ```text
   stapp03 | SUCCESS => {
       "ansible_facts": {
           "discovered_interpreter_python": "/usr/bin/python3"
       },
       "changed": false,
       "ping": "pong"
   }
   stapp01 | SUCCESS => {
       "ansible_facts": {
           "discovered_interpreter_python": "/usr/bin/python3"
       },
       "changed": false,
       "ping": "pong"
   }
   stapp02 | SUCCESS => {
       "ansible_facts": {
           "discovered_interpreter_python": "/usr/bin/python3"
       },
       "changed": false,
       "ping": "pong"
   }
   ```

4. **Create and edit the Ansible playbook:**

   Open the playbook file for editing:

   ```bash
   vi playbook.yml
   ```

   Add the following YAML configuration to install the `httpd` package:

   ```yaml
   ---
   - hosts: app_servers
     become: yes
     become_user: root
     tasks:
       - name: Install httpd package on all servers
         yum:
           name: httpd
           state: present
   ```

5. **Run the Ansible playbook:**

   Execute the playbook to apply the configuration to all specified servers:

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

   The output should confirm that the `httpd` package was installed successfully on all servers:

   ```text
   PLAY [app_servers] **********************************************************************

   TASK [Gathering Facts] ******************************************************************
   ok: [stapp03]
   ok: [stapp01]
   ok: [stapp02]

   TASK [Install httpd package on all servers] *********************************************
   ok: [stapp01]
   ok: [stapp02]
   ok: [stapp03]

   PLAY RECAP ******************************************************************************
   stapp01                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp02                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp03                    : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```
