# Problem Statement

---
Nautilus Application development team wants to test the Apache and PHP setup on one of the app servers in Stratos Datacenter. They want the DevOps team to prepare an Ansible playbook to accomplish this task. Below you can find more details about the task.

There is an inventory file *~/playbooks/inventory* on *jump host*.

Create a playbook ~*/playbooks/httpd.yml* on *jump host* and perform the following tasks on *App Server 1.*

a. Install *httpd* and *php* packages (whatever default version is available in yum repo).

b. Change default document root of Apache to */var/www/html/myroot* in default Apache config */etc/httpd/conf/httpd.conf*. Make sure */var/www/html/myroot* path exists (if not please create the same).

c. There is a template ~*/playbooks/templates/phpinfo.php.j2* on **jump host**. Copy this template to the Apache document root you created as *phpinfo.php* file and make sure user owner and the group owner for this file is *apache* user.

d. Start and enable *httpd* service.

Note: Validation will try to run the playbook using command *ansible-playbook -i inventory httpd.yml*, so please make sure the playbook works this way without passing any extra arguments.

---

## Solution:


1. **Navigate to the Playbooks Directory:**

   ```bash
   cd ~/playbooks
   ```



2. **Inspect Inventory File:**

   ```bash
   cat inventory
   ```

   ```plaintext
     stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
     stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
     stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
   ```

3. **Inspect Templates Directory:**

   ```bash
   cd templates/
   ls
    phpinfo.php.j2
   ```

4. **Create the Playbook:**

   Create the playbook file `httpd.yml` in the `~/playbooks/` directory with the following content:

   ```yaml
   - hosts: stapp01
     become: yes
     tasks:
     - name: Install Packages
       package:
         name:
           - httpd
           - php
         state: latest

     - name: Create document root
       file:
         path: /var/www/html/myroot
         state: directory
         group: apache
         owner: apache
         mode: '0755'

     - name: Change the document root
       replace:
         path: /etc/httpd/conf/httpd.conf
         regexp: 'DocumentRoot "/var/www/html"'
         replace: 'DocumentRoot "/var/www/html/myroot"'

     - name: Copy template phpinfo.php.j2
       ansible.builtin.template:
         src: phpinfo.php.j2
         dest: /var/www/html/myroot/phpinfo.php
         owner: apache
         group: apache

     - name: Start and enable the httpd service
       service:
         name: httpd
         state: started
         enabled: true
   ```

5. **Execute the Playbook:**

   Run the playbook using the following command:

   ```bash
   ansible-playbook -i inventory httpd.yml
   ```

**Validation of Output:**

Upon successful execution, you should see output indicating that all tasks have completed successfully. The expected output should be:

```plaintext
PLAY [stapp01] **************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp01]

TASK [Install Packages] *****************************************************************
ok: [stapp01]

TASK [Create document root] *************************************************************
changed: [stapp01]

TASK [Change the document root] *********************************************************
changed: [stapp01]

TASK [Copy template phpinfo.php.j2] *****************************************************
changed: [stapp01]

TASK [Start and enable the httpd service] ***********************************************
changed: [stapp01]

PLAY RECAP ******************************************************************************
stapp01                    : ok=6    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

