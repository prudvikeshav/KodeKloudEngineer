# Problem Statement

The *Nautilus* system admins team has rolled out a web UI application for their backup utility on the *Nautilus backup* server within the *Stratos Datacenter*. This application operates on port *8084*, and *firewalld* is active on the server. To meet operational needs, the following requirements have been identified:

Allow all incoming connections on port *8084/tcp*. Ensure the zone is set to *public*.

## Solution

---

### 1. Connect to the Server

1. **SSH into the Server:**

   ```bash
   ssh clint@stbkp01
   ```

2. **Switch to Root User:**

   ```bash
   sudo su
   ```

### 2. Configure *firewalld* to Allow Port 8084

1. **Add Port 8084 to the Public Zone:**

   ```bash
   firewall-cmd --add-port=8084/tcp --zone=public --permanent
   ```

2. **Reload *firewalld* to Apply Changes:**

   ```bash
   sudo firewall-cmd --reload
   ```

### 3. Verify the Configuration

1. **Check the Open Ports in the Public Zone:**

   ```bash
   sudo firewall-cmd --zone=public --list-ports
   ```

2. **Check the Active Zones:**

   ```bash
   sudo firewall-cmd --get-active-zones
   ```

   - Verifies that the public zone is active and applied to the network interfaces.

---
