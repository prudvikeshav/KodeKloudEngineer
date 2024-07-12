#### Problem Statement

#### *One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:*

#### *a. Create an image games:nautilus on Application Server 2 from a container ubuntu_latest that is running on same server.*

### Solution

#### list the container running

```bash
docker container ls
```

#### To Create a new image from a container's changes

```bash
docker container commit 164a6eba532f games:nautilus
```

#### Output

```bash
[root@stapp02 steve]# docker images
REPOSITORY   TAG        IMAGE ID       CREATED         SIZE
games        nautilus   d40de38957ad   6 seconds ago   115MB
ubuntu       latest     35a88802559d   4 weeks ago     78MB
```
