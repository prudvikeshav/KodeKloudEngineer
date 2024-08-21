
## Problem Statement

The Nautilus DevOps team has confidential data on App Server 3 in the Stratos Datacenter. A container named _ubuntu_latest_ is running on the same server. Your task is to copy an encrypted file _/tmp/nautilus.txt.gpg_ from the Docker host to the _ubuntu_latest_ container, placing it in the _/tmp/_ directory. It is crucial to ensure that the file remains unchanged during this transfer process.

## Solution

To achieve this, follow these steps:

1. **Copy the File from the Docker Host to the Container**

   Use the `docker cp` command to transfer the encrypted file from the Docker host to the specified path in the _ubuntu_latest_ container. This command ensures that the file is copied directly and accurately without any modifications.

   ```bash
   docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp
   ```

2. **Verify the File Transfer**

   After copying the file, you can confirm its presence and ensure the transfer was successful by accessing the container and listing the contents of the target directory. Use the `docker exec` command to open an interactive bash shell inside the container.

   ```bash
   docker exec -it ubuntu_latest /bin/bash
   ```

   Once inside the container, navigate to the _/tmp/_ directory and check for the presence of the file:

   ```bash
   cd /tmp/
   ls
   ```

   Expected output:

   ```
   nautilus.txt.gpg
   ```

   This output confirms that the file _nautilus.txt.gpg_ has been successfully copied to the container and is located in the specified directory.

---
