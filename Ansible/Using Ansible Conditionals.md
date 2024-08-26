# Problem Statement

---
The Nautilus DevOps team had a discussion about, how they can train different team members to use Ansible for different automation tasks. There are numerous ways to perform a particular task using Ansible, but we want to utilize each aspect that Ansible offers. The team wants to utilise Ansible's conditionals to perform the following task:

An inventory file is already placed under /home/thor/ansible directory on jump host, with all the Stratos DC app servers included.

Create a playbook /home/thor/ansible/playbook.yml and make sure to use Ansible's when conditionals statements to perform the below given tasks.

Copy blog.txt file present under /usr/src/dba directory on jump host to App Server 1 under /opt/dba directory. Its user and group owner must be user tony and its permissions must be 0755 .

Copy story.txt file present under /usr/src/dba directory on jump host to App Server 2 under /opt/dba directory. Its user and group owner must be user steve and its permissions must be 0755 .

Copy _media.txt_ file present under /usr/src/dba directory on jump host to App Server 3 under /opt/dba directory. Its user and group owner must be user banner and its permissions must be 0755.

NOTE: You can use *ansible_nodename*variable from gathered facts with _when_ condition. Additionally, please make sure you are running the play for all hosts i.e use _- hosts: all_.

Note: Validation will try to run the playbook using command _ansible-playbook -i inventory playbook.yml_, so please make sure the playbook works this way without passing any extra arguments.



```yaml
- hosts: all
  become: yes
  tasks:
   -  name:  Copy file blog.txt to stapp01
      ansible.builtin.copy:
          src: /usr/src/dba/blog.txt
          dest: /opt/dba
          state: present
          owner: tony
          group: tony
          mode: "0755"
      when: inventory_hostname == "stapp01"
    
    -  name:  Copy file story.txt to stapp02
      ansible.builtin.copy:
          src: /usr/src/dba/story.txt
          dest: /opt/dba
          state: present
          owner: steve
          group: steve
          mode: "0755"
      when: inventory_hostname == "stapp02"

-  name:  Copy file media.txt to stapp03
      ansible.builtin.copy:
          src: /usr/src/dba/media.txt
          dest: /opt/dba
          state: present
          owner: banner
          group: banner
          mode: "0755"
      when: inventory_hostname == "stapp03"
