# Problem Statement:

### *One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 2 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:*
### *a. Install apache2 in kkloud container using apt that is running on App Server 2 in Stratos Datacenter.*
### *b. Configure Apache to listen on port 3000 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.*
### *c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.*



# Solution:

### Check the container
```bash
[root@stapp02 steve]# docker container ls
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS     NAMES
19dc0db308b7   ubuntu:18.04   "/bin/bash"   14 minutes ago   Up 14 minutes             kkloud
```
### Login into the container
```bash
docker container exec -it 19dc0 bin/bash
```
### Install Apache2 service
```bash
apt install apache2
```
### Change the port no from 80 to 3000
```bash
vi ports.conf

/etc/apache2/sites-enabled/000-default.conf

Listen 3000
Listen 80

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
### Restart the apache2 service
```bash
root@19dc0db308b7:/etc/apache2# service apache2 restart
 * Restarting Apache httpd web server apache2
```

