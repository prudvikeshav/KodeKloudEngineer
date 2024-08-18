# Problem statement

---

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:

a. Create an inventory file _/home/thor/ansible/inventory_ on _jump_host_ and add all application servers as managed nodes.

b. Create a playbook _/home/thor/ansible/playbook.yml_ on the _jump host_ to copy the _/usr/src/itadmin/index.html_ file to all application servers, placing it at _/opt/itadmin_.

Note: Validation will run the playbook using the command _ansible-playbook -i inventory playbook.yml_. Ensure the playbook functions properly without any extra arguments.

## Solution

---

### 1. Create the Inventory File

1. **Navigate to the `ansible` directory:**

   ```bash
   cd /home/thor/ansible/
   ```

2. **Create or edit the `inventory` file:**

   ```bash
   vi inventory
   ```

3. **Add the following content to the inventory file:**

   ```ini
   [app_servers]
   stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
   stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
   stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n
   ```

   This inventory file defines a group called `app_servers` with three application servers.

### 2. Create the Ansible Playbook

Create a playbook at `/home/thor/ansible/playbook.yml` to copy the file from the jump host to the application servers.

   ```bash
   vi playbook.yml
   ```

#### **Add the following content to the playbook file**

   ```yaml
   ---
   - hosts: app_servers
     become: yes
     tasks:
       - name: Copy index.html on all application servers
         copy:
           src: /usr/src/itadmin/index.html
           dest: /opt/itadmin
   ```

   This playbook will copy the `index.html` file from `/usr/src/itadmin/` on the jump host to `/opt/itadmin/` on each application server. The `become: yes` directive ensures that the file is copied with elevated privileges.

### 3. Run the Playbook

To execute the playbook and copy the file to all application servers:

1. **Run the playbook using `ansible-playbook`:**

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

2. **Review the output:**

   You should see output similar to this if everything is configured correctly:

   ```plaintext
   PLAY [app_servers] ********************************************************************

   TASK [Gathering Facts] **************************************************************
   ok: [stapp01]
   ok: [stapp02]
   ok: [stapp03]

   TASK [Copy index.html on all application servers] **********************************
   changed: [stapp01]
   changed: [stapp02]
   changed: [stapp03]

   PLAY RECAP **************************************************************************
   stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```
