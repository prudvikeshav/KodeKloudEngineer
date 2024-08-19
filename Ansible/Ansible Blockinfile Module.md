# Problem Statement

---
The Nautilus DevOps team wants to install and set up a simple _httpd_ web server on all app servers in Stratos DC. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, write the required playbook to complete this task. Find more details about the task below.

- We already have an _inventory_ file under _/home/thor/ansible_ directory on _jump host_. Create a _playbook.yml_ under _/home/thor/ansible_ directory on _jump host_ itself.

- Using the playbook, install _httpd_ web server on all app servers. Additionally, make sure its service should up and running.

- Using _blockinfile_ Ansible module add some content in _/var/www/html/index.html_ file. Below is the content:

_Welcome to XfusionCorp!_

_This is  Nautilus sample file, created using Ansible!_

_Please do not modify this file manually!_

- The _/var/www/html/index.html_ file's user and group _owner_ should be _apache_ on all app servers.

- The _/var/www/html/index.html_ file's permissions should be _0777_ on all app servers.

Note:

i. Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure the playbook works this way without passing any extra arguments.

ii. Do not use any custom or empty _marker_ for _blockinfile_ module.

---

## Solution

1. **Navigate to the Ansible Directory**

   ```bash
   cd /home/thor/ansible
   ```

2. **Check the Inventory File**

   ```bash
   cat inventory
   ```

     ```plaintext
     stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
     stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
     stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
     ```

3. **Create the Playbook File**

   ```bash
   vi playbook.yml
   ```

   **Playbook Content**:

   ```yaml
   ---
   - hosts: all
     become: yes
     tasks:
     - name: Install httpd on all servers
       yum:
         name: httpd
         state: present
       # This task installs the 'httpd' package on all app servers using the 'yum' package manager.
       
     - name: Start httpd service
       service:
         name: httpd
         state: started
       # This task ensures that the 'httpd' service is started on all app servers.
     
     - name: Insert content into /var/www/html/index.html
       ansible.builtin.blockinfile:
         path: /var/www/html/index.html
         create: yes
         group: "apache"
         owner: "apache"
         mode: "0777"
         block: |
           Welcome to XfusionCorp!
           This is a Nautilus sample file, created using Ansible!
           Please do not modify this file manually!
       # This task creates or updates the '/var/www/html/index.html' file with the specified content,
       # sets the file ownership to 'apache', and sets permissions to '0777'.
   ```

4. **Run the Ansible Playbook**

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

5. **Review the Output**

   ```
   PLAY [all] ******************************************************************************

   TASK [Gathering Facts] ******************************************************************
   ok: [stapp02]
   ok: [stapp03]
   ok: [stapp01]

   TASK [Install httpd on all servers] *****************************************************
   ok: [stapp02]
   ok: [stapp01]
   ok: [stapp03]

   TASK [Start httpd service] **************************************************************
   ok: [stapp02]
   ok: [stapp03]
   ok: [stapp01]

   TASK [Insert content into /var/www/html/index.html] *****************************************************
   changed: [stapp01]
   changed: [stapp03]
   changed: [stapp02]

   PLAY RECAP ******************************************************************************
   stapp01                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   stapp02                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   stapp03                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```
