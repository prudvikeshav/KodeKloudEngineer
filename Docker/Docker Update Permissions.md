# Problem Statement:

### *One of the Nautilus project developers need access to run docker commands on App Server 2. This user is already created on the server. Accomplish this task as per details given below:*

###*User rose is not able to run docker commands on App Server 2 in Stratos DC, make the required changes so that this user can run docker commands without sudo.*


# Solution:

### Check if docker Wheel group
```bash
groupadd docker
```
### Add user rose to the group
```bash
gpasswd -a rose docker
```
### switch to user rose
```bash
su rose
```
# Output
```bash
[rose@stapp02 steve]$ docker image ls -a
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

[rose@stapp02 steve]$ docker pull  nginx
Using default tag: latest
latest: Pulling from library/nginx
f11c1adaa26e: Pull complete 
c6b156574604: Pull complete 
ea5d7144c337: Pull complete 
1bbcb9df2c93: Pull complete 
537a6cfe3404: Pull complete 
767bff2cc03e: Pull complete 
adc73cb74f25: Pull complete 
Digest: sha256:67682bda769fae1ccf5183192b8daf37b64cae99c6c3302650f6f8bf5f0f95df
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```