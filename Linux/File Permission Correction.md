# Problem Statement
After conducting a security audit within the Stratos DC, the Nautilus security team discovered misconfigured permissions on critical files. To address this, corrective actions are being taken by the production support team. Specifically, the file named /etc/hosts on Nautilus App 1 server requires adjustments to its Access Control Lists (ACLs) as follows:

1. The file's user owner and group owner should be set to *root*.

2. *Others* should possess *read only* permissions on the file.

3. User *kirsty* must not have any permissions on the file.

4. User *jerome* should be granted *read only* permission on the file.


## Solution:


#### **1. Connect to Nautilus App 1 Server**

Log in to the server as the user `tony`:

```bash
ssh tony@stapp01
```

#### **2. Switch to the Root User**



```bash
sudo su
```

#### **3. Check Current ACLs and Permissions**

Verify the current ACLs on `/etc/hosts`:

```bash
getfacl /etc/hosts
```

- **Output Example:**

  ```
  getfacl: Removing leading '/' from absolute path names
  # file: etc/hosts
  # owner: root
  # group: root
  user::rw-
  group::r--
  other::r--
  ```

#### **4. Ensure the File Ownership is Correct**



```bash
chown root:root /etc/hosts
```


#### **5. Create the User `kirsty`**

```
 id kristy
 
 #  id: ‘kristy’: no such user
```

```bash
useradd kirsty
```



#### **6. Set ACLs on the File**

1. **Remove All Permissions for `kirsty`**:

   ```bash
   setfacl -m u:kirsty:0 /etc/hosts
   ```

 

2. **Grant Read-Only Permissions to `jerome`**:

   ```bash
   setfacl -m u:jerome:r /etc/hosts
   ```

 

#### **7. Verify the Updated ACLs**



```bash
getfacl /etc/hosts
```

- **Expected Output:**

  ```
  getfacl: Removing leading '/' from absolute path names
  # file: etc/hosts
  # owner: root
  # group: root
  user::rw-
  user:jerome:r--
  user:kirsty:---
  group::r--
  mask::r--
  other::r--
  ```

