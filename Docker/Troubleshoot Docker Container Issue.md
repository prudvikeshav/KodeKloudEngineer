# Problem Statement:

### *An issue has arisen with a static website running in a container named nautilus on App Server 1. To resolve the issue, investigate the following details:*

### *Check if the container's volume /usr/local/apache2/htdocs is correctly mapped with the host's volume /var/www/html.*

### *Verify that the website is accessible on host port 8080 on App Server 1. Confirm that the command curl http://localhost:8080/ works on App Server 1.*

# Solution:

### Check the container. 
```bash
docker container ls
```
### If the container is not in running state.
```bash
docker container  ls -a
```
### Start the container.
```bash
docker conatiner start ba847
```
### Check the website is accessible.
```bash
curl http://localhost:8080/
```





