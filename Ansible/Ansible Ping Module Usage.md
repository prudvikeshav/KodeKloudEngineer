# Problem Statement

---
The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in Stratos DC. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. _Jump host_ is our Ansible controller, and we are going to run Ansible playbooks through thor user from _jump host_.

b. There is an inventory file /home/thor/ansible/inventory on _jump host_. Using that inventory file test _Ansible ping_ from _jump host_ to _App Server 3_, make sure ping works.

## Solution

1. **Review the Inventory File:**
   - Start by checking the existing inventory file to see the current configuration of the app servers. The inventory file is located at `/home/thor/ansible/inventory` on the _jump host_.

   ```bash
   cd /home/thor/ansible
   cat inventory
   ```

   The initial content of the inventory file will be:

   ```
   stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n
   stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@
   stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n
   ```

   Here, the `ansible_user` parameter is missing.

2. **Edit the Inventory File:**
   - Open the inventory file in a text editor to add the missing `ansible_user` parameter. You need to specify the `ansible_user` for `stapp02`.

   ```bash
   vi inventory
   ```

   Locate the entry for `stapp02` and update it to include the `ansible_user` parameter. For instance, if the user should be `steve`, modify the line as follows:

   ```
   stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
   ```

   Save the changes and exit the editor

3. **Test Ansible Connectivity:**
   - After updating the inventory file, verify that Ansible can successfully connect to `stapp02` with the new configuration. Run the following command to ping `stapp02`:

   ```bash
   ansible stapp02 -m ping -i inventory -v
   ```

   If the configuration is correct, you should receive a successful response, indicating that Ansible can communicate with the server:

   ```
   Using /etc/ansible/ansible.cfg as config file
   stapp02 | SUCCESS => {
       "ansible_facts": {
           "discovered_interpreter_python": "/usr/bin/python3"
       },
       "changed": false,
       "ping": "pong"
   }
   ```

   This confirms that the `ansible_user` and password are correctly set, and Ansible is able to establish a connection.
