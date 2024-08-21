
## Problem Statement

One of the DevOps team members has created a custom Docker image named `cluster:devops` on App Server 1 in the Stratos Datacenter. The image is now saved on this server. A new requirement has been raised to use this image for testing on App Server 3. The task is to:

1. Save the `cluster:devops` image as an archive on App Server 1.
2. Transfer the image archive to App Server 3.
3. Load the image archive on App Server 3 with the same name and tag.

**Note:** Docker is already installed on both servers. If the Docker service is down, ensure it is started.

## Solution

### 1. Save the Docker Image on App Server 1

- **Log in to App Server 1** and check the existing Docker images to confirm the presence of the `cluster:devops` image.

  ```bash
  docker image list
  ```

  Expected output:

  ```
  REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
  cluster      devops    d5dc2018d4da   4 minutes ago   116MB
  ubuntu       latest    35a88802559d   4 weeks ago     78MB
  ```

- **Save the Docker image** to a tar file. This command creates a tarball of the image that can be transferred to another server.

  ```bash
  docker image save cluster:devops -o cluster.tar
  ```

  This will create a file named `cluster.tar` containing the `cluster:devops` Docker image.

### 2. Transfer the Image Archive to App Server 3

- **Copy the tar file** to App Server 3 using SCP (Secure Copy Protocol). Ensure you have appropriate permissions to access `/tmp` on App Server 3.

  ```bash
  scp /home/tony/cluster.tar banner@stapp03:/tmp
  ```

  Here, `/home/tony/cluster.tar` is the path to the tar file on App Server 1, and `/tmp` is the destination path on App Server 3.

### 3. Load the Docker Image on App Server 3

- **Log in to App Server 3** and load the Docker image from the tar file into Docker.

  ```bash
  docker image load -i /tmp/cluster.tar
  ```

  This command imports the image from the tar file into Docker, making it available for use on App Server 3.

- **Verify the image** on App Server 3 to ensure it has been loaded correctly.

  ```bash
  docker image list
  ```

  Expected output:

  ```
  REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
  cluster      devops    d5dc2018d4da   8 minutes ago   116MB
  ```

  The output should show the `cluster:devops` image with the same tag and ID as on App Server 1.
