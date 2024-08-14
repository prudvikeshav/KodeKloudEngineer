## Problem Statement

The Nautilus DevOps team aims to manage all servers within the stack using Ansible, utilizing a common sudo user across all servers. They plan to use this user for various tasks on each server. While this isn't finalized, they're starting with testing. Ansible is already installed on the jump host via yum. Here's the requirement:

On the jump host, modify the default configuration of Ansible to enable the use of anita as the default SSH user for all hosts. Ensure to make changes within Ansible's default configuration without creating a new one.

## Solution

### 1. Switch to Superuser

You need to have superuser privileges to modify the Ansible configuration file located at `/etc/ansible/ansible.cfg`. Use the `sudo` command to switch to the superuser account.

```bash
sudo su
```

After running this command, you will be prompted to enter your password. Once authenticated, you will have superuser privileges.

### 2. Locate and Edit the Default Configuration File

#### Open the Configuration File

```bash
vi /etc/ansible/ansible.cfg
```

#### Modify the Configuration

     Add or update the `remote_user` setting under the `[defaults]` section to specify `anita` as the default SSH user.

   ```ini
   [defaults]
   remote_user = anita
   ```
