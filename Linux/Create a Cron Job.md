# Problem Statement

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

a. Install *cronie* package on all Nautilus app servers and start crond service.

b. Add a cron */5* ** * echo hello > /tmp/cron_text for root user.

## Solution

### 1. SSH into the First App Server

```bash
ssh tony@stapp01
```

### 2. Switch to the Root User

```bash
sudo su
```

### 3. Install the *cronie* Package

```bash
yum install cronie -y
```

### 4. Start the `crond` Service

```bash
systemctl start crond
systemctl enable crond
```

### 5. Edit the Root User’s Crontab

Open the crontab editor for the root user:

```bash
crontab -e
```

In the editor, add the following line to schedule the job to run every 5 minutes:

```bash
*/5 * * * * echo hello > /tmp/cron_text
```

### 6. Verify the Crontab Entry

```bash
crontab -l
```

You should see the following line in the output:

```bash
*/5 * * * * echo hello > /tmp/cron_text
```
