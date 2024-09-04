# Problem Statement

In the Stratos Datacenter, our *Storage server* is encountering performance degradation due to excessive processes held by the *nfsuser* user. To mitigate this issue, we need to enforce limitations on its maximum processes. Please set the maximum process limits as specified below:

a. Set the soft limit to *1025*

b. Set the hard limit to *2026*

## Solution

---

### 1. Connect to the Server

1. **SSH into the Server:**

   ```bash
   ssh natasha@ststor01
   ```

2. **Switch to Root User:**

   ```bash
   sudo su
   ```

### 2. Verify the `nfsuser` User

1. **Check the `nfsuser` ID:**

   ```bash
   id nfsuser
   ```

### 3. Configure Process Limits

1. **Edit the Limits Configuration File:**

   ```bash
   vi /etc/security/limits.conf
   ```

2. **Add or Modify Limits for `nfsuser`:**
   - Add the following lines to set the soft and hard limits for the `nfsuser`:

     ```plain
     #@student        -       maxlogins       4
     nfsuser          soft    nproc           1025
     nfsuser          hard    nproc           2026
     ```

### 4. Verify the Changes

 ```plain
 ulimit -u
 ```
