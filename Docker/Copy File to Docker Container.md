## Problem Statement

 The Nautilus DevOps team possesses confidential data on App Server 3 in the Stratos Datacenter. A container named _ubuntu_latest_ is running on the same server.

Copy an encrypted file _/tmp/nautilus.txt.gpg_ from the docker host to the _ubuntu_latest_ container located at _/tmp/_. Ensure the file is not modified during this operation.

## Solution

Copy file from local to container

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp
```

To check the file copied or not

```bash
docker exec -it ubuntu_latest /bin/bash
```

Output:

```
cd /tmp/
tmp# ls
nautilus.txt.gpg
```
