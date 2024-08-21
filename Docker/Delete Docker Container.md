
## Problem Statement

A container named `kke-container` was created by one of the Nautilus project developers on App Server 1 for testing purposes. As it is no longer needed, the container needs to be deleted. Complete the following task: Delete the `kke-container` on App Server 1 in the Stratos Datacenter.

## Solution

To delete the `kke-container`, follow these steps:

1. **List Running Containers**

   Begin by listing all active containers to confirm that `kke-container` exists and check its current state (running or stopped). Use the `docker container ls` command to display a list of running containers.

   ```bash
   docker container ls
   ```

2. **Stop the Container (if it is running)**

   If `kke-container` is currently running, it must be stopped before it can be deleted. Use the `docker container stop` command to stop the container.

   ```bash
   docker container stop kke-container
   ```

   This command will stop the `kke-container` gracefully, allowing it to be removed.

3. **Delete the Container**

   Once the container is stopped (or if it was already stopped), proceed to delete it using the `docker container rm` command. This command removes the specified container from the Docker host.

   ```bash
   docker container rm kke-container
   ```

   This command will delete `kke-container` from App Server 1, freeing up resources and cleaning up the Docker environment.
