
# Problem Statement

In response to heightened security concerns, the xFusionCorp Industries security team has opted for custom Apache users for their web applications. Each user is tailored specifically for an application, enhancing security measures. Your task is to create a custom Apache user according to the outlined specifications:

a. Create a user named _john_ on _App server 3_ within the Stratos Datacenter.

b. Assign a unique UID _1321_ and designate the home directory as _/var/www/john_.

---

## Solution

Here's an enhanced description of the solution for creating a custom Apache user with specific requirements on an application server:

### Steps to Create and Configure the User

1. **Log in to App Server 3:**
   First, connect to the application server using SSH. Execute the following command from your local machine:

   ```bash
   ssh banner@stapp03
   ```

   Here, `banner` is the username you use to access the server, and `stapp03` is the hostname of App Server 3.

2. **Switch to the Root User:**
   Once logged in, escalate your privileges to the root user to perform administrative tasks. Use the following command:

   ```bash
   sudo su
   ```

   This command will prompt you for the root password or your user password, depending on your sudo configuration.

3. **Create the User with a Specific UID:**
   With root privileges, create a new user named _john_ and assign a unique user ID (UID) of 1321. Run the following command:

   ```bash
   useradd -u 1321 john
   ```

   The `-u` option specifies the UID, and `john` is the username. By default, this command creates a home directory for the user at `/home/john`.

4. **Set the Custom Home Directory:**
   Change the default home directory to `/var/www/john`. This step is necessary to align with the specified directory structure for web applications. Use the following command:

   ```bash
   usermod -d /var/www/john john
   ```

   The `-d` option updates the user's home directory path.

5. **Verify the User Configuration:**
   To ensure that the user _john_ has been created with the correct UID and home directory, check the user information in the `/etc/passwd` file. Execute the following command:

   ```bash
   grep john /etc/passwd
   ```

   You should see an output similar to:

   ```
   john:x:1321:1321::/var/www/john:/bin/bash
   ```

   In this output:
   - `john` is the username.
   - `x` indicates that the password is managed through shadow files.
   - `1321` is the UID and GID (group ID).
   - `/var/www/john` is the home directory.
   - `/bin/bash` is the default shell.
