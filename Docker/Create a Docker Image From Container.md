
## Problem Statement

A Nautilus developer needs to back up changes made to a container as part of their testing process. The DevOps team has been requested to create a new Docker image based on these changes. Specifically, you need to create an image named `games:nautilus` from the `ubuntu_latest` container that is running on Application Server 2.

## Solution

To create a Docker image from the modified container and back up its changes, follow these steps:

1. **List Running Containers**

   First, identify the container you want to create an image from by listing all running containers. Use the `docker container ls` command to view active containers and their IDs.

   ```bash
   docker container ls
   ```

2. **Create a New Image from the Container**

   Once you have the container ID (e.g., `164a6eba532f`), use the `docker container commit` command to create a new image. This command captures the current state of the container and creates a new image with the specified name and tag.

   ```bash
   docker container commit 164a6eba532f games:nautilus
   ```

   Here, `164a6eba532f` is the container ID, and `games:nautilus` is the name and tag for the new image.

3. **Verify the New Image**

   After creating the image, confirm that it has been successfully created by listing all available Docker images. Use the `docker images` command to check for the presence of the newly created image.

   ```bash
   docker images
   ```

   Expected output:

   ```
   REPOSITORY   TAG        IMAGE ID       CREATED         SIZE
   games        nautilus   d40de38957ad   6 seconds ago   115MB
   ubuntu       latest     35a88802559d   4 weeks ago     78MB
   ```

   This output shows the `games:nautilus` image along with its ID and creation timestamp, indicating that the image was created successfully.
