# Problem Statement

---
There is some data on all app servers in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Write a playbook.yml under _/home/thor/ansible_ on jump host, an inventory is already present under _/home/thor/ansible_ directory on Jump host itself. Perform below given tasks using this playbook:

We have a file _/opt/devops/blog.txt_ on app server 1. Using Ansible _replace_ module replace string _xFusionCorp_ to _Nautilus_ in that file.

We have a file _/opt/devops/story.txt_ on app server 2. Using Ansible _replace_ module replace the string _Nautilus_ to _KodeKloud_ in that file.

We have a file _/opt/devops/media.txt_ on app server 3. Using Ansible _replace_ module replace string _KodeKloud_ to _xFusionCorp Industries_ in that file.

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
-  hosts: all
   become: yes
   tasks:
   - name: Replace string in /opt/devops/blog.txt on stapp01
     replace:
        path: /opt/devops/blog.txt
        regexp: 'xFusionCorp'
        replace: 'Nautilus'
     when: inventory_hostname == 'stapp01'

   - name: Replace string in /opt/devops/story.txt on stapp02
     replace:
        path: /opt/devops/story.txt
        regexp: 'Nautilus'
        replace: 'KodeKloud'
     when: inventory_hostname == 'stapp02'

   - name: Replace string in /opt/devops/media.txt on stapp03
     replace:
        path: /opt/devops/media.txt
        regexp: 'KodeKloud'
        replace: 'xFusionCorp Industries'
     when: inventory_hostname == 'stapp03'
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
ok: [stapp03]
ok: [stapp01]
ok: [stapp02]

TASK [Replace string in /opt/devops/blog.txt on stapp01] ********************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Replace string in /opt/devops/story.txt on stapp02] *******************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Replace string in /opt/devops/media.txt on stapp03] *******************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP ******************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```
