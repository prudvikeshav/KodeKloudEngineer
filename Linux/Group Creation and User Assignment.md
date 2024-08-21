# Problem Statement

The system admin team at xFusionCorp Industries has streamlined access management by implementing group-based access control. Here's what you need to do:

a. Create a group named *nautilus_admin_users* across all App servers within the Stratos Datacenter.

b. Add the user *rajesh* into the *nautilus_admin_users* group on all App servers. If the user doesn't exist, create it as well.

## Solution

### Task A: Create Group nautilus_admin_users Across All App Servers

1. **Log into each App server** in the Stratos Datacenter. This can be done via SSH or any other remote access method used.

2. **Create the group** `nautilus_admin_users` on each server:

   ```bash
   sudo groupadd nautilus_admin_users
   ```

   Verify that the group was created:

   ```bash
   cat /etc/group | grep nautilus_admin_users
   ```

### Task B: Add User `rajesh` to `nautilus_admin_users` Group on All App Servers

1. **Check if user `rajesh` exists** on each server:

   ```bash
   id rajesh
   ```

   - If the user does not exist, you need to create it:

     ```bash
     sudo useradd rajesh
     ```

   - If the user exists, you can skip to adding the user to the group.

2. **Add `rajesh` to the `nautilus_admin_users` group**:

   ```bash
   sudo usermod -aG nautilus_admin_users rajesh
   ```

   Verify that the user was added to the group:

   ```bash
   id rajesh
   ```

   OutPut:

   ```plain
   uid=1002(rajesh) gid=1002(nautilus_admin_users) groups=1002(nautilus_admin_users)
   ```
