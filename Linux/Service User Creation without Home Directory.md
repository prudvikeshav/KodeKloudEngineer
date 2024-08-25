In response to the latest tool implementation at xFusionCorp Industries, the system admins require the creation of a service user account. Here are the specifics:

- Create a user named _mariyam_ in _App Server 2_ without a home directory.


## Solution:

1. **SSH into the Server and Switch to Root:**

   ```bash
   ssh steve@stapp02
   sudo su
   ```

2. **Create the User Without a Home Directory:**
   - Use the `-M` option with `useradd` to ensure no home directory is created.

   ```bash
   useradd -M mariyam
   ```

3. **Verify User Creation:**
   - Check if the user has been created and verify the details.

   ```bash
   id mariyam
   ```

   Output should be:
     ```
      uid=1002(mariyam) gid=1002(mariyam) groups=1002(mariyam)
     ```


4. **Verify User's Entry in `/etc/passwd`:**
   - Check the `/etc/passwd` file to ensure the user's home directory is not set.

   ```bash
   cat /etc/passwd | grep mariyam
   ```
       Output should be:

    ```
      mariyam:x:1002:1002::/home/mariyam:/bin/bash
    ```
