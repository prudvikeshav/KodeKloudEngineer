# Problem Statement

---
One of the DevOps team members has created a zip archive on _jump host_ in Stratos DC that needs to be extracted and copied over to all app servers in Stratos DC itself. Because this is a routine task, the Nautilus DevOps team has suggested automating it. We can use Ansible since we have been using it for other automation tasks. Below you can find more details about the task:

- We have an _inventory_ file under _/home/thor/ansible_ directory on _jump host_, which should have all the app servers added already.

- There is a zip archive _/usr/src/data/devops.zip_ on _jump host_.

- Create a _playbook.yml_ under _/home/thor/ansible/_ directory on _jump host_ itself to perform the below given tasks.

- Unzip _/usr/src/data/devops.zip_ archive in _/opt/data/_ location on all app servers.

- Make sure the extracted data must has the respective sudo user as their _user_ and _group_ owner, i.e tony for app server 1, steve for app server 2, banner for app server 3.

- The extracted data permissions must be _0644_.

_Note_: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure playbook works this way, without passing any extra arguments.

Certainly! Here's the enhanced description for each step of the solution:

## Solution

---

1. **Navigate to the Ansible Directory**

   ```bash
   cd ansible/
   ```

2. **Check the Inventory**

   ```bash
   cat inventory 
   ```

     ```plaintext
     stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
     stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
     stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
     ```

3. **Check the Presence of the Zip File**

   ```bash
   ls /usr/src/devops/
   ```

4. **Create the Playbook File**

   ```
   vi playbook.yml
   ```

   ```yaml
   
   ---
   - hosts: all
     become: yes
     tasks:
     - name: Unzip archive /usr/src/data/devops.zip
       unarchive:
            src: /usr/src/devops/datacenter.zip
            dest: /opt/data/
            group: "{{ ansible_user }}"
            owner: "{{ ansible_user }}"
            mode: "0644"
   ```

5. **Run the Playbook**

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

6. **Review the Output**

   ```
   PLAY [all] ******************************************************************************

   TASK [Gathering Facts] ******************************************************************
   ok: [stapp03]
   ok: [stapp02]
   ok: [stapp01]

   TASK [Unzip archive /usr/src/data/devops.zip] *******************************************
   changed: [stapp03]
   changed: [stapp02]
   changed: [stapp01]

   PLAY RECAP ******************************************************************************
   stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```
