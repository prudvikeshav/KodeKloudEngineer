# Problem Statement

##

### *The Nautilus DevOps team is testing applications containerization, which is supposed to be migrated on docker container-based environments soon. In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:*

### *a. On App Server  3 in Stratos DC pull nginx image (preferably latest tag but others should work too).*

### *b. Create a new container with name **official** from the image you just pulled.*

### *c. Map the host volume **/opt/itadmin** with container volume **/usr/src/**. There is an sample.txt file present on same server under **/tmp**; copy that file to **/opt/itadmin**. Also please keep the container in running state.*

# Solution

## Docker Pull nginx:latest

```bash
docker pull nginx:latest
```

### TO bind a host volume with container volume  

```bash
docker run -it -d --name official -v /opt/itadmin:/usr/src/ nginx:latest
```

## TO inspect wheather the binding successful

```bash
docker inspect 7fcd

"Mounts": [
            {
                "Type": "bind",
                "Source": "/opt/itadmin",
                "Destination": "/usr/src",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
```

## Copy the sample.txt from /tmp to /opt/itadmin/

```bash
mv /tmp/sample.txt /opt/itadmin/
```

## To check wheather the sample.txt accessable from container volume

```bash
docker exec -it 7fcd /bin/bash

root@7fcdf15f33d2:/# cat /usr/src/sample.txt 
This is a sample file!!root@7fcdf15f33d2:/# 
```
