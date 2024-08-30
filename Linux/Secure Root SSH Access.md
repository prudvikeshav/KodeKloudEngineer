
# Problem Statement

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.

## Solution

To address this requirement, we need to automate the process of disabling direct root SSH login on multiple servers. The solution involves the following steps:

1. **Define Server Details**: Create arrays to store the server names, SSH usernames, and corresponding passwords.

2. **Iterate Through Servers**: Loop through each server, using the respective username and password to perform SSH operations.

3. **Disable Root Login**:
   - **Connect to Server**: Use `sshpass` to manage SSH login with passwords, avoiding manual intervention.
   - **Update SSH Configuration**: Modify the SSH configuration file (`/etc/ssh/sshd_config`) to disable root login. This involves:
     - Commenting out any existing `PermitRootLogin` directives.
     - Adding or updating the `PermitRootLogin no` directive to restrict root access.
   - **Restart SSH Service**: Apply the changes by restarting the SSH service to ensure that the new configuration takes effect.

4. **Error Handling and Reporting**: Check the success or failure of each operation and provide appropriate feedback.

Here is the enhanced script that implements the above solution:

```bash
#!/bin/bash

# Define lists of servers, usernames, and passwords
servers=("stapp01" "stapp02" "stapp03")
users=("tony" "steve" "banner")
passwords=("Ir0nM@n" "Am3ric@" "BigGr33n")

# Loop through each server to apply changes
for i in "${!servers[@]}"; do
    server="${servers[$i]}"
    user="${users[$i]}"
    password="${passwords[$i]}"

    echo "Processing $server..."

    # SSH into the server and execute commands
    sshpass -p "$password" ssh -o StrictHostKeyChecking=no "$user@$server" <<EOF
        # Disable root login in the SSH configuration
        echo "$password" | sudo -S bash -c "
            # Backup the current SSH configuration
            cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

            # Disable root login
            sed -i 's/^#PermitRootLogin .*/PermitRootLogin no/' /etc/ssh/sshd_config
            sed -i 's/^PermitRootLogin .*/PermitRootLogin no/' /etc/ssh/sshd_config

            # Restart SSH service to apply changes
            systemctl restart sshd
        "
EOF

    # Check the exit status of the SSH command
    if [ $? -eq 0 ]; then
        echo "Successfully disabled root login on $server"
    else
        echo "Failed to update $server"
    fi

    echo "---------------------------------"
done
```

### Explanation of the Script

1. **Variables**:
   - `servers`, `users`, and `passwords` arrays store the necessary details for each server.

2. **SSH Operations**:
   - `sshpass -p "$password"`: Manages password-based SSH authentication.
   - `ssh -o StrictHostKeyChecking=no "$user@$server"`: Connects to each server, bypassing host key checks for automation purposes.

3. **Commands Execution**:
   - **Backup Configuration**: Creates a backup of the current SSH configuration file to safeguard against potential issues.
   - **Modify Configuration**: Updates the SSH configuration to disable root login.
   - **Restart SSH Service**: Ensures that the new configuration is applied immediately.

4. **Error Handling**:
   - Checks the success of each SSH command and reports accordingly.

**Security Considerations**:

- **Password Handling**: The script uses passwords directly, which could be a security risk. Consider using SSH keys and configuring `sudo` for passwordless access to specific commands for a more secure solution.
- **Testing**: Before deploying the script in a production environment, thoroughly test it in a controlled setting to verify its functionality and avoid unintended disruptions.
