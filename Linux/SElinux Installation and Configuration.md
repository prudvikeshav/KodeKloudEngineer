# Problem Statement

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for *App server 2* in the Stratos Datacenter:

Install the required *SELinux* packages.

Permanently disable *SELinux* for the time being; it will be re-enabled after necessary configuration changes.

No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

Disregard the current status of SELinux via the command line; the final status after the reboot should be *disabled*.

## Solution

---

### 1. Connect to the Server

1. **SSH into the Server:**

   ```bash
   ssh steve@stapp02
   ```

2. **Switch to Root User:**

   ```bash
   sudo su
   ```

### 2. Update the System

1. **Update All Packages:**

   ```bash
   sudo yum update
   ```

### 3. Verify SELinux Package Installation

1. **Check for Existing SELinux Packages:**

   ```bash
   sudo rpm -aq | grep selinux
   ```

.

2.**Install Required SELinux Packages:**

   ```bash
   sudo yum install policycoreutils policycoreutils-python setools setools-console setroubleshoot
   ```

### 4. Configure SELinux to be Disabled

1. **Edit SELinux Configuration File:**

   ```bash
   vi /etc/selinux/config
   ```

2. **Modify Configuration Settings:**

      ```plain
     # This file controls the state of SELinux on the system.
     # SELINUX= can take one of these three values:
     #     enforcing - SELinux security policy is enforced.
     #     permissive - SELinux prints warnings instead of enforcing.
     #     disabled - No SELinux policy is loaded.
     SELINUX=disabled
     # SELINUXTYPE= can take one of three values:
     #     targeted - Targeted processes are protected,
     #     minimum - Modification of targeted policy. Only selected processes are protected.
     #     mls - Multi Level Security protection.
     SELINUXTYPE=targeted
     ```

### 5. Verify Configuration

1. **Check SELinux Status:**

   ```bash
   sudo sestatus
   ```
