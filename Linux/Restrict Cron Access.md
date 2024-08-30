# Problem Statement

In alignment with security compliance standards, the Nautilus project team has opted to impose restrictions on crontab access. Specifically, only designated users will be permitted to create or update cron jobs.

Configure crontab access on*App Server 1* as follows: Allow crontab access to *mariyam* user while denying access to the *eric* user.

Here's a detailed step-by-step solution to configure crontab access on *App Server 1* to allow the `mariyam` user while denying access to the `eric` user.

## Solution

1. **Log in to App Server 1:**

   ```bash
   ssh tony@stapp01
   ```

2. **Switch to the root user:**

   ```bash
   sudo su
   ```

3. **Allow crontab access for `mariyam`:**

   ```bash
   echo "mariyam" >> /etc/cron.allow
   ```

4. **Deny crontab access for `eric`:**

   ```bash
   echo "eric" >> /etc/cron.deny
   ```

5. **Verify the configuration:**

   ```bash
   cat /etc/cron.allow
   cat /etc/cron.deny
   ```
