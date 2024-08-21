
## Problem Statement

A developer named `rose` needs to run Docker commands on App Server 2 in the Stratos Datacenter. The user `rose` is already created on the server but is currently unable to execute Docker commands without using `sudo`. You need to make the necessary changes to allow this user to run Docker commands directly.

## Solution

To enable the user `rose` to run Docker commands without `sudo`, follow these steps:

1. **Check for the Docker Group**

   First, ensure that the Docker group exists on the server. The Docker group allows users to run Docker commands without requiring root privileges.

   ```bash
   groupadd docker
   ```

   If the Docker group already exists, this command will have no effect. If it doesn’t exist, this command will create it.

2. **Add the User to the Docker Group**

   Add the user `rose` to the Docker group. This grants the user the necessary permissions to run Docker commands without `sudo`.

   ```bash
   gpasswd -a rose docker
   ```

   This command modifies the group membership for `rose`, adding them to the `docker` group.

3. **Switch to the User `rose`**

   After adding `rose` to the Docker group, switch to the `rose` user account to verify the changes.

   ```bash
   su rose
   ```

   You will need the password for the `rose` user to switch to their account.

4. **Verify Docker Command Execution**

   As the `rose` user, check if Docker commands can be executed without `sudo`. You can test this by listing Docker images or pulling a Docker image.

   - **List Docker Images:**

     ```bash
     docker image ls -a
     ```

     Expected output (example):

     ```
     REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
     ```

   - **Pull a Docker Image:**

     ```bash
     docker pull nginx
     ```

     Expected output (example):

     ```
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
