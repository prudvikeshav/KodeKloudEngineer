# Problem Statement

---
As part of the temporary assignment to the Nautilus project, a developer named anita requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

Create a user named *anita* on *App Server 3* in Stratos Datacenter. Set the expiry date to *2024-02-17*, ensuring the user is created in lowercase as per standard protocol.

---

## Solution

---

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
   With root privileges, create a new user named *anita* and assign a expiry date to *2024-02-17*.
    Run the following command:

   ```bash
       useradd anita -e 2024-02-17
   ```

4. **Verify the User Configuration:**
    To ensure that the user *anita* has been created with the correct expiry date, check the user information by *chage* command. Execute the following command:

   ```bash
    chage -l anita
   ```

   OutPut:

    ```plain
    Last password change                                    : Aug 26, 2024
    Password expires                                        : never
    Password inactive                                       : never
    Account expires                                         : Feb 17, 2024
    Minimum number of days between password change          : 0
    Maximum number of days between password change          : 99999
    Number of days of warning before password expires       : 7
    ```
