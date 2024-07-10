# Problem Statement

### On Application Server 3 create a container named nginx_3 using the nginx image with the alpine tag. Ensure container is in a running state.

# Solution

### *Creating a container nginx_3 from image nginx:alpine*
```shell
docker run -itd --name=nginx_3 nginx:alpine
```