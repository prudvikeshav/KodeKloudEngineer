# Problem  Statement

---
There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

- Create a playbook named _playbook.yml_ under _/home/thor/ansible_ directory on jump host, an _inventory_ file is already present under _/home/thor/ansible_ directory on Jump Server itself.

- Create an empty file blog.txt under _/opt/sysops/_ directory on app server 1. Set some _acl_ properties for this file. Using _acl_ provide _read '(r)'_ permissions to group _tony_ (i.e _entity_ is _tony_ and _etype_ is group).

- Create an empty file story.txt under _/opt/sysops/_ directory on app server 2. Set some _acl_ properties for this file. Using _acl_ provide _read + write '(rw)_' permissions to user _steve_ (i.e _entity_ is _steve_ and _etype_ is user).

- Create an empty file _media.txt_ under _/opt/sysops/_ on app server 3. Set some _acl_ properties for this file. Using _acl_ provide _read + write '(rw)_' permissions to group _banner_ (i.e _entity_ is _banner_ and _etype_ is group).

_Note_: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_ so please make sure the playbook works this way, without passing any extra arguments.

## Solution

---

### Step 1: Navigate to the Ansible Directory

Change to the directory where your Ansible playbook and inventory file will reside.

```bash
cd /home/thor/ansible
```

### Step 2: Check the Inventory File

Verify the contents of the `inventory` file to ensure it has the correct details for your app servers.

```bash
cat inventory
```

You should see something like this:

```
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

### Step 3: Create the Ansible Playbook

Create a new file named `playbook.yml` using a text editor such as `vi` or `nano`.

```bash
vi playbook.yml
```

### Step 4: Write the Playbook

In the editor, add the following content to the `playbook.yml` file. This playbook will perform the tasks as specified in your requirements:

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Create an empty file blog.txt on app server 1
      file:
        path: /opt/sysops/blog.txt
        state: touch
      when: inventory_hostname == 'stapp01'
    
    - name: Set ACL to grant read access to group 'tony' on blog.txt
      ansible.posix.acl:
        path: /opt/sysops/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present
      when: inventory_hostname == 'stapp01'
    
    - name: Create an empty file story.txt on app server 2
      file:
        path: /opt/sysops/story.txt
        state: touch
      when: inventory_hostname == 'stapp02'
    
    - name: Set ACL to grant read and write access to user 'steve' on story.txt
      ansible.posix.acl:
        path: /opt/sysops/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present
      when: inventory_hostname == 'stapp02'
    
    - name: Create an empty file media.txt on app server 3
      file:
        path: /opt/sysops/media.txt
        state: touch
      when: inventory_hostname == 'stapp03'
    
    - name: Set ACL to grant read and write access to group 'banner' on media.txt
      ansible.posix.acl:
        path: /opt/sysops/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present
      when: inventory_hostname == 'stapp03'
```

### Step 5: Run the Ansible Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

### Step 6: Verify the Output

```
PLAY [all] ******************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [create  to a file blog.txt.] ******************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Grant ACL properties to user Tony read access to a file blog.txt] *****************
skipping: [stapp02]
skipping: [stapp03]
ok: [stapp01]

TASK [create  to a file story.txt.] *****************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Grant ACL properties to usergrpup  Steve readand write  access to a file story.txt.] ***
skipping: [stapp01]
skipping: [stapp03]
ok: [stapp02]

TASK [create  to a file media.txt.] *****************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

TASK [Grant ACL properties user Banner read and write  access to a file media.txt.] *****
skipping: [stapp01]
skipping: [stapp02]
ok: [stapp03]

PLAY RECAP ******************************************************************************
stapp01                    : ok=3    changed=1    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
stapp02                    : ok=3    changed=1    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
stapp03                    : ok=3    changed=1    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
```
