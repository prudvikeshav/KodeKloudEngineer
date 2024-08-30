# Problem Statement

A Nautilus developer has stored confidential data on the jump host within Stratos DC. To ensure security and compliance, this data must be transferred to one of the app servers. Given developers lack direct access to these servers, the system admin team has been enlisted for assistance.

Copy */tmp/nautilus.txt.gpg* file from jump server to *App Server 3* placing it in the directory */home/opt*.

## Solution

### **1. Transfer the File Using `scp`**

Log in to the jump host and use `scp` to copy the file to `App Server 3`. Make sure to replace the placeholders with the actual usernames, hostnames, and paths:

```bash
scp /tmp/nautilus.txt.gpg banner@stapp03:/home/opt
```

- **Explanation**:
  - `scp`: Securely copies files between hosts on a network.
  - `/tmp/nautilus.txt.gpg`: The path to the file on the jump host.
  - `banner@stapp03:/home/opt`: The destination path on `App Server 3`.

**Expected Output**:

```plaintext
nautilus.txt.gpg                                       100%  105   148.7KB/s   00:00
```

#### **2. Verify the File Transfer on `App Server 3`**

Log in to `App Server 3` to check if the file has been transferred successfully:

```bash
ssh banner@stapp03
```

- **Explanation**: SSH into `App Server 3` as user `banner`.

Once logged in, list the contents of the target directory to ensure the file is there:

```bash
ls -l /home/opt/
```

**Expected Output**:

```plaintext
-rw-r--r-- 1 banner banner 105 Aug 27 10:00 nautilus.txt.gpg
```
