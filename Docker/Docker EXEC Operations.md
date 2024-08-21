
## Problem Statement

A Nautilus DevOps team member was configuring services on a `kkloud` container running on App Server 2 in the Stratos Datacenter. This team member is currently on PTO for the rest of the week, and the remaining configuration tasks need to be completed as soon as possible. The tasks are as follows:

- Install Apache2 in the `kkloud` container using `apt`.
- Configure Apache to listen on port 3000 instead of the default HTTP port. Ensure Apache listens on all network interfaces (localhost, 127.0.0.1, container IP, etc.), not just a specific IP or hostname.
- Ensure the Apache service is running and the container remains in a running state at the end.

## Solution

1. **Identify the Container**

   Start by listing all running containers to locate the `kkloud` container and obtain its container ID.

   ```bash
   docker container ls
   ```

   Example output:

   ```
   CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS     NAMES
   19dc0db308b7   ubuntu:18.04   "/bin/bash"   14 minutes ago   Up 14 minutes             kkloud
   ```

   Here, `19dc0db308b7` is the container ID of the `kkloud` container.

2. **Access the Container**

   Use the `docker container exec` command to open a bash shell inside the `kkloud` container.

   ```bash
   docker container exec -it 19dc0db308b7 /bin/bash
   ```

3. **Install Apache2**

   Inside the container, install Apache2 using the `apt` package manager.

   ```bash
   apt update
   apt install -y apache2
   ```

   The `-y` flag automatically confirms the installation prompts.

4. **Configure Apache to Listen on Port 3000**

   Modify Apache’s configuration to change the listening port from the default port 80 to port 3000. Edit the necessary configuration files:

   - Open the `ports.conf` file:

     ```bash
     vi /etc/apache2/ports.conf
     ```

     Update it to:

     ```apache
     Listen 3000
     ```

   - Open the default site configuration file:

     ```bash
     vi /etc/apache2/sites-enabled/000-default.conf
     ```

     Ensure the file contains:

     ```apache
     <VirtualHost *:3000>
         # Other directives here
     </VirtualHost>
     ```

     Remove or comment out any lines specifying port 80.

5. **Restart Apache Service**

   After making the changes, restart the Apache2 service to apply the new configuration.

   ```bash
   service apache2 restart
   ```

   Ensure Apache restarts without issues and begins listening on port 3000.

6. **Verify Apache Status**

   To confirm that Apache is running and listening on the new port, you can use the following command inside the container:

   ```bash
   netstat -tuln | grep 3000
   ```

   This will show if Apache is listening on port 3000.
