# Problem Statement

---
The Nautilus DevOps team is practicing some of the Ansible modules and creating and testing different Ansible playbooks to accomplish tasks. Recently they started testing an Ansible file module to create soft links on all app servers. Below you can find more details about it.

- Write a _playbook.yml_ under _/home/thor/ansible_ directory on _jump host_, an _inventory_ file is already present under _/home/thor/ansible_ directory on _jump host_ itself. Using this playbook accomplish below given tasks:

- Create an empty file _/opt/dba/blog.txt_ on app server 1; its user owner and group owner should be _tony_. Create a _symbolic link_ of source path _/opt/dba_ to destination _/var/www/html_.

- Create an empty file _/opt/dba/story.txt_ on app server 2; its user owner and group owner should be _steve_. Create a _symbolic link_ of source path _/opt/dba_ to destination _/var/www/html_.

- Create an empty file _/opt/dba/media.txt_ on app server 3; its user owner and group owner should be banner. Create a _symbolic link_ of source path _/opt/dba_ to destination _/var/www/html_.

Note: Validation will try to run the playbook using command _ansible-playbook -i inventory_ playbook.yml so please make sure playbook works this way without passing any extra arguments.

---

## Solution

---

### Step 1: Navigate to the Ansible Directory

1. **Open a terminal** on the jump host.
2. **Change directory** to `/home/thor/ansible`:

   ```bash
   cd /home/thor/ansible
   ```

### Step 2: Create the Ansible Playbook

1. **Create a new file** named `playbook.yml`:

   ```bash
   vi playbook.yml
   ```

3. **Add the following content** to `playbook.yml`:

   ```yaml
   ---
   - hosts: all
     become: yes
     tasks:
       - name: Create an empty file on stapp01
         file:
           path: /opt/dba/blog.txt
           state: touch
           owner: tony
           group: tony
         when: inventory_hostname == 'stapp01'

       - name: Create an empty file on stapp02
         file:
           path: /opt/dba/story.txt
           state: touch
           owner: steve
           group: steve
         when: inventory_hostname == 'stapp02'

       - name: Create an empty file on stapp03
         file:
           path: /opt/dba/media.txt
           state: touch
           owner: banner
           group: banner
         when: inventory_hostname == 'stapp03'
       
       - name: Create a symbolic link on all the app_servers
         ansible.builtin.file:
           src: /opt/dba/
           dest: /var/www/html
           state: link
   ```

### Step 3: Validate the Inventory File

**Ensure the `inventory` file** is correctly set up in `/home/thor/ansible` directory. It should contain:

   ```ini
   stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
   stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
   stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
   ```

### Step 4: Run the Ansible Playbook

**Execute the playbook** using the `ansible-playbook` command:

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

**Expected Output**

```plain

PLAY [all] ******************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp03]
ok: [stapp01]
ok: [stapp02]

TASK [Create an empty file /opt/dba/blog.txt on stapp03] *****************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Create an empty file /opt/dba/story.txt on stapp02] ****************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Create an empty file /opt/dba/media.txt on stapp03] ****************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

TASK [Create a symbolic link on all the app_servers] ************************************
changed: [stapp02]
changed: [stapp01]
changed: [stapp03]

PLAY RECAP ******************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

```