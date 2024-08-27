# Problem Statement

In the daily standup, it was noted that the timezone settings across the Nautilus Application Servers in the Stratos Datacenter are inconsistent with the local datacenter's timezone, currently set to *America/Argentina/Ushuaia*.

Synchronize the timezone settings to match the local datacenter's timezone (*America/Argentina/Ushuaia*).

## Solution

1. **SSH into Each Server**

   Log in to each server using SSH. Repeat this step for all servers.

   ```bash
   ssh tony@stapp01
   ```

2. **Switch to the Root User**

   After logging in, switch to the root user to have the necessary privileges to change the timezone.

   ```bash
   sudo su
   ```

3. **Set the Timezone**

   Use the `timedatectl` command to set the timezone to `America/Argentina/Ushuaia`.

   ```bash
   timedatectl set-timezone 'America/Argentina/Ushuaia'
   ```

4. **Verify the Timezone Change**

   Check that the timezone has been updated correctly.

   ```bash
   timedatectl
   ```

   You should see an output similar to this:

   ```
   Local time: Tue 2024-08-27 09:33:23 -03
   Universal time: Tue 2024-08-27 12:33:23 UTC
   RTC time: n/a
   Time zone: America/Argentina/Ushuaia (-03, -0300)
   System clock synchronized: yes
   NTP service: n/a
   RTC in local TZ: no
   ```

5. **Log Out of the Root Session**

   Exit from the root and server user session.

   ```bash
   exit
   ```

7. **Repeat for All Servers**

   Perform the above steps on all relevant servers (e.g., `stapp02`, `stapp03`). Ensure that each server has the timezone set to `America/Argentina/Ushuaia`.
