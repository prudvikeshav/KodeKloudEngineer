## 🧩 Problem Statement

The system administration team at **xFusionCorp Industries** needs to provision a user (`anita`) for a backup process. This user:

- Should **exist** on App Server 1
- Should **not have login privileges**
- Must use a **non-interactive shell** (`/sbin/nologin`)

---

## ✅ Solution Steps

### 1. SSH into App Server 1

```bash
ssh tony@stapp01
```
### 2. Create User with Non-Interactive Shell
```bash

sudo useradd anita -s /sbin/nologin
```
This command creates the user and explicitly sets the shell to /sbin/nologin, preventing direct login.

### 3. Verify the User Configuration
Use cat to confirm the shell is correctly assigned:

```bash
cat /etc/passwd | grep anita
```
#### Sample Output:
```bash
anita:x:1002:1002::/home/anita:/sbin/nologin
```

### 🔐 Why Use /sbin/nologin?

#### Prevents interactive shell access

#### Ensures user can run background or automated tasks (e.g., backup jobs)

#### Meets principle of least privilege for system/service accounts
