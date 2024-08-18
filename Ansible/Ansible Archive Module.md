# Problem Statement

---
The Nautilus DevOps team has some data on each app server in Stratos DC that they want to copy to a different location. However, they want to create an archive of the data first, then they want to copy the same to a different location on the respective app server. Additionally, there are some specific requirements for each server. Perform the task using Ansible playbook as per requirements mentioned below:

Create a playbook named _playbook.yml_ under _/home/thor/ansible_ directory on _jump host_, an _inventory_ file is already placed under _/home/thor/ansible_/ directory on _Jump Server_ itself.

Create an archive _demo.tar.gz_ (make sure archive format is _tar.gz_) of /_usr/src/security/_ directory ( present on each app server ) and copy it to _/opt/security/_ directory on all _app servers_. The user and group _owner_ of archive _demo.tar.gz_ should be _tony_ for _App Server 1_, _steve_ for _App Server 2_ and _banner_ for _App Server 3_.

---

## Solution

---

1. **Navigate to the Ansible directory:**

   ```bash
   cd /home/thor/ansible/
   ```

2. **Review the inventory file:**

   Check the inventory file to confirm it lists all the app servers with their details:

   ```bash
   cat inventory
   ```

   The content should look like this:

   ```ini
   stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
   stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
   stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
   ```

3. **Create and edit the playbook:**

   Open or create the `playbook.yml` file in your preferred text editor:

   ```bash
   vi playbook.yml
   ```

   Add the following YAML configuration to create an archive and set the owner for each server:

   ```yaml
   ---
   - hosts: all
     become: yes
     tasks:
       - name: Create an archive demo.tar.gz
         archive:
           path: /usr/src/security/
           dest: /opt/security/demo.tar.gz
           format: gz
         
       - name: Set owner of the archive for stapp01
         file:
           path: /opt/security/demo.tar.gz
           owner: tony
           group: tony
         when: inventory_hostname == 'stapp01'

       - name: Set owner of the archive for stapp02
         file:
           path: /opt/security/demo.tar.gz
           owner: steve
           group: steve
         when: inventory_hostname == 'stapp02'

       - name: Set owner of the archive for stapp03
         file:
           path: /opt/security/demo.tar.gz
           owner: banner
           group: banner
         when: inventory_hostname == 'stapp03'
   ```

   This configuration does the following:
   - Creates a `tar.gz` archive of the `/usr/src/security/` directory and places it in `/opt/security/`.
   - Sets the owner and group of the archive based on the host.

4. **Run the Ansible playbook:**

   Execute the playbook using the following command:

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

   The output should confirm the successful execution of the tasks:

   ```text
   PLAY [all] ******************************************************************************

   TASK [Gathering Facts] ******************************************************************
   ok: [stapp01]
   ok: [stapp02]
   ok: [stapp03]

   TASK [Create an archive demo.tar.gz] ****************************************************
   changed: [stapp01]
   changed: [stapp02]
   changed: [stapp03]

   TASK [Set owner of the archive for stapp01] *********************************************
   changed: [stapp01]

   TASK [Set owner of the archive for stapp02] *********************************************
   changed: [stapp02]

   TASK [Set owner of the archive for stapp03] *********************************************
   changed: [stapp03]

   PLAY RECAP ******************************************************************************
   stapp01                    : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp02                    : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp03                    : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```
