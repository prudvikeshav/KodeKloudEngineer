## Problem Statement

### *One of the DevOps team members was working on to create a new custom docker image on App Server 1 in Stratos DC. He is done with his changes and image is saved on same server with name cluster:devops. Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on App Server 3. So we need to provide them that image on App Server 3 in Stratos DC.*

### *a. On App Server 1 save the image **cluster:devops** in an archive.*

### *b. Transfer the image archive to App Server 3.*

### *c. Load that image archive on App Server 3 with same name and tag which was used on App Server 1.*

### *Note: Docker is already installed on both servers; however, if its service is down please make sure to start it.*

## Solution

### Login into appserver1 and check for images

```bash
[root@stapp01 tony]# docker image list
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
cluster      devops    d5dc2018d4da   4 minutes ago   116MB
ubuntu       latest    35a88802559d   4 weeks ago     78MB
```

### Save the docker image into tar file

```bash
 docker image save cluster:devops -o cluster.tar
 ```

### Copy the tar file to App server3

 ```bash
 scp /home/tony/cluster.tar banner@stapp03:/tmp
 ```

### Load the image

 ```bash
  docker image load -i /tmp/cluster.tar 
 ```

## Output

```bash
[root@stapp03 banner]# docker image list
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
cluster      devops    d5dc2018d4da   8 minutes ago   116MB
```
