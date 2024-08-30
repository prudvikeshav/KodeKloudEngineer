# Problem Statement

With the installation of new tools on the app servers within the Stratos Datacenter, certain functionalities now necessitate graphical user interface (GUI) access.

Adjust the default runlevel on all App servers in Stratos Datacenter to enable GUI booting by default. It's imperative not to initiate a server reboot after completing this task.

## Solution

1. **SSH into the First Server**

   ```bash
   ssh tony@stapp01
   ```

2. **Switch to Root User**

   After logging in, switch to the root user:

   ```bash
   sudo su
   ```

3. **Check the Current Default Target**

   Verify the current default target to see if it needs to be changed:

   ```bash
   systemctl get-default
   ```

4. **Change the Default Target to Graphical**

   If the current default target is not `graphical.target`, set it as the default:

   ```bash
   systemctl set-default graphical.target
   ```

   You should see a message like:

   ```plain
   Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
   ```

5 . **Repeat for the Remaining Servers**

   Perform steps 1 through 6 on the remaining servers (`stapp02` and `stapp03`), using the appropriate usernames and passwords:

   For `stapp02`,`stapp03` :

   ```bash
   ssh steve@stapp02
   sudo su
   systemctl get-default
   systemctl set-default graphical.target
   exit
   exit
   ```
