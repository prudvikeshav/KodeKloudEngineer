## Problem Statement

An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:

The inventory file _/home/thor/ansible/inventory_ requires adjustments. The playbook must run on _App Server 1_ in _Stratos DC_. Update the inventory accordingly.

Create a playbook _/home/thor/ansible/playbook.yml_. Include a task to create an empty file _/tmp/file.txt_ on _App Server 1_.

Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook works without any additional arguments.

## Solution

First, update the `/home/thor/ansible/inventory` file to include `App Server 1` under the `Stratos DC` group.

```ini
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
```

### Step 2: Create the Playbook

Create the playbook at `/home/thor/ansible/playbook.yml` with the following content:

```yaml
---
- name: Create an empty file on App Server 1
  hosts: stapp01
  tasks:
    - name: Ensure /tmp/file.txt exists
      file:
        path: /tmp/file.txt
        state: touch
```

### Step 3: Validate the Configuration

Ensure the playbook and inventory file are correctly placed and configured. To validate that everything is set up correctly, run:

```bash
ansible-playbook -i /home/thor/ansible/inventory /home/thor/ansible/playbook.yml
```

This command will execute the playbook using the inventory file you've adjusted and ensure that the file `/tmp/file.txt` is created on `App Server 1`.
