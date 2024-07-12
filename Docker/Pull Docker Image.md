### Problem Statement

#### *Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:*

#### *a. Pull busybox:musl image on App Server 1 in Stratos DC and re-tag (create new tag) this image as busybox:blog.*

### Solution

#### *To Pull a docker image busybox:musl*

```bash
docker pull busybox:musl
```

#### *To Tag the pulled image*

```bash
docker tag busybox:musl  busybox:blog
```

#### Output

```bash
[root@stapp01 tony]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      blog      615b080b9dbe   13 months ago   1.45MB
busybox      musl      615b080b9dbe   13 months ago   1.45MB
```
