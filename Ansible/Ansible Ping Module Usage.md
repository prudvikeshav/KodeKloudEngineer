## Problem Statement

---
The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in Stratos DC. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. _Jump host_ is our Ansible controller, and we are going to run Ansible playbooks through thor user from _jump host_.

b. There is an inventory file /home/thor/ansible/inventory on _jump host_. Using that inventory file test _Ansible ping_ from _jump host_ to _App Server 3_, make sure ping works.
