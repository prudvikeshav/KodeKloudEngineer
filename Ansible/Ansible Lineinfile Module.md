# Problem Statement

---
The Nautilus DevOps team want to install and set up a simple httpd web server on all app servers in Stratos DC. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.

We already have an _inventory_ file under _/home/thor/ansible_ directory on _jump_ host. Write a playbook _playbook.yml_ under _/home/thor/ansible_ directory on _jump_ host itself. Using the playbook perform below given tasks:

Install _httpd_ web server on all app servers, and make sure its service is up and running.

Create a file _/var/www/html/index.html_ with content:

_This is a Nautilus sample file, created using Ansible!_

Using_lineinfile_ Ansible module add some more content in _/var/www/html/index.html_ file. Below is the content:

_Welcome to xFusionCorp Industries!_

Also make sure this new line is added at the top of the file.

The _/var/www/html/index.html_ file's user and group owner should be _apache_ on all app servers.

The _/var/www/html/index.html_ file's permissions should be _0777_ on all app servers.

Note: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure the playbook works this way without passing any extra arguments.

## Solution

1. **Navigate to the Ansible Directory**

   ```bash
   cd /home/thor/ansible
   ```

2. **Create the Ansible Playbook File**

   ```bash
   vi playbook.yml
   ```

3. **Add the Playbook Content**

   Insert the following YAML configuration into the `playbook.yml` file:

   ```yaml
   ---
   - hosts: all
     become: yes
     tasks:
       - name: Install httpd on all servers
         yum:
           name: httpd
           state: present

       - name: Start and enable httpd service
         service:
           name: httpd
           state: started
           enabled: yes

       - name: Create /var/www/html/index.html with initial content
         copy:
           dest: /var/www/html/index.html
           content: |
             This is a Nautilus sample file, created using Ansible!
           owner: apache
           group: apache
           mode: '0777'

       - name: Add additional content to /var/www/html/index.html
         lineinfile:
           path: /var/www/html/index.html
           line: Welcome to xFusionCorp Industries!
           insertbefore: BOF
           create: yes
   ```

5. **Run the Playbook**

   Execute the following command to run the playbook using the inventory file:

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

### Expected Output

Upon running the playbook, you should see output similar to the following:

```
PLAY [all] ******************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Install httpd on all servers] *****************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Start and enable httpd service] *****************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Create /var/www/html/index.html with initial content] *******************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Add additional content to /var/www/html/index.html] *******************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP ******************************************************************************
stapp01                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
stapp02                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
stapp03                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
