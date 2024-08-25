# Problem Statement

To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task:

Create a user named _james_ with a _non-interactive shell_ on _App Server 2_.

## Solution

1. **SSH into the Server:**
   - Connect to `App Server 2` using SSH.

   ```bash
   ssh steve@stapp02
   ```

2. **Switch to the Root User:**
   - After connecting, switch to the root user to have the necessary permissions for creating a new user.

   ```bash
   sudo su
   ```

3. **Verify the User Doesn't Exist:**
   - Check if the user `james` already exists.

   ```bash
   id james
   id: ‘james’: no such user
   ```

4. **Create the User with a Non-Interactive Shell:**
   - Use the `useradd` command to create the user `james` with the `/sbin/nologin` shell. This shell prevents the user from logging in interactively.

   ```bash
   useradd james -s /sbin/nologin
   ```

5. **Verify User Creation:**
   - Check that the user has been created and verify the details.

   ```bash
   id james
   ```

   You should see output similar to:

   ```
   uid=1002(james) gid=1002(james) groups=1002(james)
   ```

6. **Verify the User's Shell in `/etc/passwd`:**

   ```bash
   cat /etc/passwd | grep james
   ```

   The output should be:

   ```
   james:x:1002:1002::/home/james:/sbin/nologin
   ```
