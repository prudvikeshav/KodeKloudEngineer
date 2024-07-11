## Problem Statement

### *The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 3 in Stratos Datacenter. Please perform the task as per details mentioned below*

### *a. Pull nginx:alpine-perl docker image on Application Server 3.*

### *b. Create a container named demo using the image you pulled.*

### *c. Map host port 8089 to container port 80. Please keep the container in running state.*

## Solution

### Docker pull image nginx:alpine-perl

```bash
docker pull nginx:alpine-perl
```

### Docker conatiner demo with hostport 8089, container port 80

```bash
 docker run -d -it --name=demo -p 8089:80 nginx:alpine-perl
 ```
