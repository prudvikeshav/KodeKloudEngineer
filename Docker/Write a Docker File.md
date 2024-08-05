
## Problem Statement

The Nautilus application development team requires custom Docker images for a project. You need to create a Dockerfile on App Server 1 in Stratos Datacenter to build an image with the following specifications:

- Use `ubuntu` as the base image.
- Install `apache2` and configure it to listen on port `8082` (without modifying any other Apache configuration settings like document root).

## Solution

### 1. Create the Dockerfile

Create a Dockerfile with the required configuration at `/opt/docker/Dockerfile` on App Server 1.

**Dockerfile Contents:**

```dockerfile
# Use the official Ubuntu image as the base
FROM ubuntu

# Update package lists and install apache2
RUN apt update && apt install -y apache2

# Configure Apache to listen on port 8082
RUN sed -i 's/80/8082/g' /etc/apache2/ports.conf

# Set Apache to run in the foreground
ENTRYPOINT ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
```

**Explanation:**

- `FROM ubuntu`: This sets the base image to Ubuntu.
- `RUN apt update && apt install -y apache2`: Updates the package lists and installs the Apache2 web server.
- `RUN sed -i 's/80/8082/g' /etc/apache2/ports.conf`: Changes the default port from `80` to `8082` in the Apache configuration file `ports.conf`.
- `ENTRYPOINT ["/usr/sbin/apachectl", "-D", "FOREGROUND"]`: Configures Apache to run in the foreground, which is necessary for it to stay running inside the container.

### 2. Build the Docker Image

Once the Dockerfile is created, build the Docker image using the following command:

```bash
docker build -t apache:test /opt/docker
```

**Explanation:**

- `docker build -t apache:test /opt/docker`: This command builds a Docker image from the Dockerfile located in `/opt/docker`. The `-t apache:test` option tags the image with the name `apache` and the tag `test`.

### 3. Verify the Image

To ensure the image was created correctly, you can list the Docker images and check for `apache:test`:

```bash
docker images
```

Expected output:

```
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
apache       test      <IMAGE_ID>     <TIME>          <SIZE>
```

This output confirms that the image has been successfully built and tagged.
