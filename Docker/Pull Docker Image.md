
## Problem Statement

The Nautilus project developers are preparing to test a new project in a containerized environment. They need to work with specific Docker images and have requested the following task to be completed:

- Pull the `busybox:musl` Docker image on App Server 1 in the Stratos Datacenter.
- Re-tag this image with a new tag, `busybox:blog`.

## Solution

### 1. Pull the Docker Image

First, pull the `busybox:musl` image from Docker Hub. This image is based on the BusyBox distribution and uses the musl libc.

```bash
docker pull busybox:musl
```

This command retrieves the `busybox:musl` image and downloads it to your local Docker repository on App Server 1.

### 2. Tag the Pulled Image

Next, create a new tag for the pulled image by tagging it as `busybox:blog`. This allows you to refer to the same image using a different tag.

```bash
docker tag busybox:musl busybox:blog
```

This command assigns the tag `busybox:blog` to the image that was previously tagged as `busybox:musl`. The image ID remains the same, but you can now reference it using the new tag.

### 3. Verify the Tagging

To confirm that the image has been successfully tagged, list all Docker images and check the output.

```bash
docker images
```

Expected output:

```
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      blog      615b080b9dbe   13 months ago   1.45MB
busybox      musl      615b080b9dbe   13 months ago   1.45MB
```

In this output:

- The `busybox:blog` tag is now associated with the same image ID as `busybox:musl`.
- Both tags (`busybox:blog` and `busybox:musl`) refer to the same underlying image.
