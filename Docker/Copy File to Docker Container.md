## Problem Statement

### *The Nautilus DevOps team possesses confidential data on App Server 3 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.*

### *Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /tmp/. Ensure the file is not modified during this operation.*

## Solution

### Copy file from local to container

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp
```

### To check the file copied or not

```bash
[root@stapp03 banner]# docker exec -it ubuntu_latest /bin/bash
root@ae63fa4f5a07:/# cd /tmp/
root@ae63fa4f5a07:/tmp# ls
nautilus.txt.gpg
```
