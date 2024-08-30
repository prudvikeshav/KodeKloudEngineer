# Problem Statement

---
One of the Nautilus DevOps team members is working on to develop a role for _httpd_ installation and configuration. Work is almost completed, however there is a requirement to add a jinja2 template for _index.html_ file. Additionally, the relevant task needs to be added inside the role. The inventory file _~/ansible/inventory_ is already present on _jump host_ that can be used. Complete the task as per details mentioned below:

a. Update _~/ansible/playbook.yml_ playbook to run the _httpd_ role on _App Server 1_.

b. Create a jinja2 template _index.html.j2_ under _/home/thor/ansible/role/httpd/templates/_ directory and add a line _This file was created using Ansible on <respective server>_ (for example_ This file was created using Ansible on stapp01_ in case of App Server 1). Also please make sure not to hard code the server name inside the template. Instead, use *inventory_hostname* variable to fetch the correct value.

c. Add a task inside _/home/thor/ansible/role/httpd/tasks/main.yml_ to copy this template on _App Server 1_ under */var/www/html/index.html*. Also make sure that */var/www/html/index.html* file's permissions are *0655*.

d. The user/group owner of */var/www/html/index.html* file must be respective sudo user of the server (for example *tony* in case of *stapp01*).

Note: Validation will try to run the playbook using command *ansible-playbook -i inventory playbook.yml* so please make sure the playbook works this way without passing any extra arguments.

## Solution:



#### 1. Update the `playbook.yml` to Run the `httpd` Role on App Server 3

You need to modify the existing playbook to ensure that it runs the `httpd` role on App Server 3. Open the `playbook.yml` file and update it as follows:

```yaml
- hosts: all
  become: yes
  roles:
    - role/httpd
```

Ensure this playbook is in the `~/ansible` directory.

#### 2. Create a Jinja2 Template for `index.html`

Create the `index.html.j2` template file under the `~/ansible/role/httpd/templates/` directory. This file will use the `inventory_hostname` variable to dynamically include the server name.

```bash
mkdir -p ~/ansible/role/httpd/templates
```

Create the `index.html.j2` file:

```bash
vi ~/ansible/role/httpd/templates/index.html.j2
```

Add the following content to `index.html.j2`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
</head>
<body>
    <h1>This file was created using Ansible on {{ inventory_hostname }}</h1>
</body>
</html>
```

This template uses the `inventory_hostname` variable to fetch the server name dynamically.

#### 3. Add a Task in `main.yml` to Copy the Template

You need to add a task to the `main.yml` file within the `httpd` role to copy the `index.html.j2` template to the appropriate location on App Server 3.

Edit the `~/ansible/role/httpd/tasks/main.yml` file:

```bash
vi ~/ansible/role/httpd/tasks/main.yml
```

Add the following task to `main.yml`:

```yaml
- name: Copy index.html template to* /var/www/html/index.html*
  ansible.builtin.template:
    src: index.html.j2
    dest:* /var/www/html/index.html*
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"
    mode: '0655'
  when: inventory_hostname == "stapp03"
```


#### 4. Verify the Inventory

Ensure that your inventory file lists App Server 3 correctly. For example:

```ini
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

#### 5. Run the Ansible Playbook

Execute the playbook using the `ansible-playbook` command:

```bash
ansible-playbook -i ~/ansible/inventory ~/ansible/playbook.yml
```

#### 6. Verify the Output

```
PLAY [all] ******************************************************************************

TASK [Gathering Facts] ******************************************************************
ok: [stapp02]
ok: [stapp03]
ok: [stapp01]

TASK [role/httpd : install the latest version of HTTPD] *********************************
ok: [stapp02]
ok: [stapp03]
changed: [stapp01]

TASK [role/httpd : Start service httpd] *************************************************
ok: [stapp03]
ok: [stapp02]
changed: [stapp01]

TASK [role/httpd : Copy index.html.j2 on appserver 1] ***********************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

PLAY RECAP ******************************************************************************
stapp01                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
stapp02                    : ok=3    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
stapp03                    : ok=3    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```