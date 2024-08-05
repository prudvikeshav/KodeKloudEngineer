
## Problem Statement

An issue has arisen with a static website running in a container named `nautilus` on App Server 1. To resolve the issue, you need to:

1. **Check if the container's volume `/usr/local/apache2/htdocs` is correctly mapped with the host's volume `/var/www/html`.**
2. **Verify that the website is accessible on host port 8080 on App Server 1. Confirm that the command `curl http://localhost:8080/` works on App Server 1.**

## Solution

### 1. Check the Running State of the Container

First, verify whether the container named `nautilus` is running.

- **List the running containers** to check if `nautilus` is among them.

  ```bash
  docker container ls
  ```

  This command will display the list of running containers. Look for `nautilus` in the list.

- **If the container is not running**, check all containers, including those that are stopped, to find the `nautilus` container.

  ```bash
  docker container ls -a
  ```

  This command will list all containers, regardless of their state.

- **Start the container** if it is not running. Use the container ID or name obtained from the previous command.

  ```bash
  docker container start nautilus
  ```

  This command will start the container named `nautilus`. Replace `nautilus` with the actual container name or ID if different.

### 2. Verify the Volume Mapping

Ensure that the container's volume `/usr/local/apache2/htdocs` is correctly mapped to the host's volume `/var/www/html`.

- **Inspect the container's details** to check volume mappings.

  ```bash
  docker container inspect nautilus
  ```

  Look for the `Mounts` section in the output. You should see something similar to:

  ```json
  "Mounts": [
      {
          "Type": "bind",
          "Source": "/var/www/html",
          "Destination": "/usr/local/apache2/htdocs",
          "Mode": "",
          "RW": true,
          "Propagation": "rprivate"
      }
  ]
  ```

  Ensure that `Source` is `/var/www/html` on the host and `Destination` is `/usr/local/apache2/htdocs` in the container.

### 3. Verify Website Accessibility

Ensure that the website is accessible via the host port 8080.

- **Check the website accessibility** by running a `curl` command on App Server 1.

  ```bash
  curl http://localhost:8080/
  ```

  This command should return the content of the static website. If the website is correctly set up, you should see the HTML content or homepage of the website.
