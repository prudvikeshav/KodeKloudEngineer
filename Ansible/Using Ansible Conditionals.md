# Problem Statement

---
The Nautilus DevOps team had a discussion about, how they can train different team members to use Ansible for different automation tasks. There are numerous ways to perform a particular task using Ansible, but we want to utilize each aspect that Ansible offers. The team wants to utilise Ansible's conditionals to perform the following task:

An *inventory* file is already placed under */home/thor/ansible* directory on *jump host*, with all the Stratos DC app servers included.

Create a playbook */home/thor/ansible/playbook.yml* and make sure to use Ansible's *when* conditionals statements to perform the below given tasks.

Copy *blog.txt* file present under */usr/src/dba* directory on *jump host* to *App Server 1* under */opt/dba* directory. Its user and group *owner* must be user tony and its permissions must be *0755* .

Copy story.txt file present under */usr/src/dba* directory on *jump host* to App Server 2 under */opt/dba* directory. Its user and group *owner* must be user steve and its permissions must be *0755* .

Copy _media.txt_ file present under */usr/src/dba* directory on *jump host* to App Server 3 under */opt/dba* directory. Its user and group *owner* must be user banner and its permissions must be *0755*.

NOTE: You can use *ansible_nodename*variable from gathered facts with _when_ condition. Additionally, please make sure you are running the play for all hosts i.e use _- hosts: all_.

Note: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_, so please make sure the playbook works this way without passing any extra arguments.

## Solution:


1. **Navigate to the Ansible Directory:**

   Change to the directory where you will create your playbook.

   ```bash
   cd /home/thor/ansible
   ```

2. **Check the Inventory File:**

   Make sure your inventory file is properly set up. It should list your app servers with their respective details.

   ```bash
   cat inventory
   ```

   You should see entries like:

   ```
   stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
   stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
   stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
   ```

3. **Create the Ansible Playbook:**

   Create a new file named `playbook.yml` using your preferred text editor.

   ```bash
   vi playbook.yml
   ```

4. **Write the Playbook:**

   Add the following content to your `playbook.yml` file. This playbook includes tasks that use the `when` conditional to check the hostname and perform actions accordingly.

   ```yaml
   - hosts: all
     become: yes
     tasks:
       - name: Copy file blog.txt to stapp01
         ansible.builtin.copy:
           src: */usr/src/dba*/blog.txt
           dest: */opt/dba*/blog.txt
           *owner*: tony
           group: tony
           mode: '*0755*'
         when: inventory_hostname == "stapp01"

       - name: Copy file story.txt to stapp02
         ansible.builtin.copy:
           src: */usr/src/dba*/story.txt
           dest: */opt/dba*/story.txt
           *owner*: steve
           group: steve
           mode: '*0755*'
         when: inventory_hostname == "stapp02"

       - name: Copy file media.txt to stapp03
         ansible.builtin.copy:
           src: */usr/src/dba*/media.txt
           dest: */opt/dba*/media.txt
           *owner*: banner
           group: banner
           mode: '*0755*'
         when: inventory_hostname == "stapp03"
   ```

5. **Run the Ansible Playbook:**

   Execute the playbook with the `ansible-playbook` command.

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

6. **Verify the Output:**

   After running the playbook, check the output to ensure that the tasks were executed correctly. You should see output indicating that files were copied and permissions were set as specified.

   Example output:

   ```
   PLAY [all] ******************************************************************************

   TASK [Gathering Facts] ******************************************************************
   ok: [stapp01]
   ok: [stapp02]
   ok: [stapp03]

   TASK [Copy file blog.txt to stapp01] ****************************************************
   changed: [stapp01]

   TASK [Copy file story.txt to stapp02] ***************************************************
   changed: [stapp02]

   TASK [Copy file media.txt to stapp03] ***************************************************
   changed: [stapp03]

   PLAY RECAP ******************************************************************************
   stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
   ```


