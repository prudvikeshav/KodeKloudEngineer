## Problem Statement

The Nautilus DevOps team is preparing to host an application using an `nginx`-based container. A ticket has been assigned to set up an `nginx` container on Application Server 3 in the Stratos Datacenter. The specific tasks are as follows:

- **Pull the `nginx:alpine-perl` Docker image** on Application Server 3.
- **Create a container named `demo`** using the pulled image.
- **Map the host port `8089`** to the container port `80`.
- Ensure that the container is running at the end of the setup.

## Solution

Follow these steps to complete the setup:

1. **Pull the Docker Image**

   First, pull the `nginx:alpine-perl` image from the Docker repository. This image includes `nginx` with Perl support and is based on the Alpine Linux distribution.

   ```bash
   docker pull nginx:alpine-perl
   ```

   This command downloads the image to your local Docker environment on Application Server 3.

2. **Create and Run the Container**

   Next, create and run a new container named `demo` using the `nginx:alpine-perl` image. Map the host port `8089` to the container port `80` to make the web service accessible on port `8089` of the host.

   ```bash
   docker run -d -it --name=demo -p 8089:80 nginx:alpine-perl
   ```

   In this command:
   - `-d` runs the container in detached mode, so it operates in the background.
   - `-it` allows for interactive terminal access if needed.
   - `--name=demo` assigns the name `demo` to the container.
   - `-p 8089:80` maps port `8089` on the host to port `80` inside the container, enabling access to the `nginx` web server from the host’s port `8089`.
   - `nginx:alpine-perl` specifies the image to use for creating the container.

3. **Verify the Container is Running**

   After running the container, confirm that it is up and running by listing all active containers:

   ```bash
   docker container ls
   ```

   Look for the `demo` container in the output to ensure it is in a running state.
