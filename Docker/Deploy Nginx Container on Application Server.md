
### Problem Statement

On Application Server 3, you need to create a Docker container named `nginx_3` using the `nginx` image with the `alpine` tag. Ensure that the container is started and running.

### Solution

To create and run the `nginx_3` container with the specified image and tag, follow these steps:

1. **Create and Start the Container**

   Use the `docker run` command to create a new container from the `nginx:alpine` image. The `-itd` flags are used to run the container in interactive mode with a terminal attached, and in detached mode (in the background). The `--name` option specifies the name of the container.

   ```shell
   docker run -itd --name=nginx_3 nginx:alpine
   ```

   - `docker run`: Command to create and start a new container.
   - `-itd`: Combined flags where `-i` stands for interactive mode, `-t` allocates a pseudo-TTY, and `-d` runs the container in detached mode.
   - `--name=nginx_3`: Names the container `nginx_3`.
   - `nginx:alpine`: Specifies the image to use, which is `nginx` with the `alpine` tag.

2. **Verify the Container is Running**

   After running the command, check that the container is up and running by listing all running containers. This ensures that the container named `nginx_3` is active and operational.

   ```shell
   docker ps
   ```

   This command will show a list of all currently running containers. Look for `nginx_3` in the output to confirm that it is running.
