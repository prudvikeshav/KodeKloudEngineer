In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named *xfusioncorp.sh*. While the script has been distributed to all necessary servers, it lacks executable permissions on *App Server 3* within the Stratos Datacenter.

Your task is to grant executable permissions to the */tmp/xfusioncorp.sh* script on *App Server 3.* Additionally, ensure that all users have the capability to execute it.

# Solution:


#### **1. Connect to App Server 3**

Start by connecting to App Server 3 via SSH using the `banner` account.

```bash
ssh banner@stapp03
```


#### **2. Switch to the Superuser (Root)**

Once logged in, switch to the root user to modify the script's permissions.

```bash
sudo su
```


#### **3. Check the Current Permissions**

Verify the current permissions of the script to understand its current state.

```bash
ls -al /tmp/xfusioncorp.sh
```


#### **4. Grant Executable Permissions**

Update the permissions of the script so that all users can execute it.

```bash
chmod 755 /tmp/xfusioncorp.sh
```

- **Explanation**:
  - `chmod 755`: This command sets the permissions of the file to `rwxr-xr-x`.
    - `rwx` (read, write, execute) for the owner (root).
    - `r-x` (read, execute) for the group (root).
    - `r-x` (read, execute) for others (all users).

#### **5. Verify the Permissions**

Check the permissions again to ensure that they have been updated correctly.

```bash
ls -al /tmp/xfusioncorp.sh
```


#### **6. Execute the Script**

Run the script to ensure it works as expected.

```bash
. /tmp/xfusioncorp.sh
```

