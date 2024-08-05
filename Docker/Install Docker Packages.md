
## Problem Description

The Nautilus DevOps team is preparing to containerize various applications. Following a recent meeting with the application development team, the goal is to set up Docker on App Server 2 for testing purposes. This includes:

- Installing `docker-ce` and `docker-compose` packages.
- Starting the Docker service to ensure it is operational.

## Solution

### 1. Install `yum-utils`

First, install `yum-utils`, which provides utilities to manage Yum repositories and packages.

```shell
sudo yum install -y yum-utils
```

### 2. Install Docker Packages

Add the Docker repository to the Yum package manager, and then install Docker and Docker Compose packages.

- **Add Docker Repository**

  ```shell
  sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  ```

  This command configures Yum to use the Docker repository for installation.

- **Install Docker and Docker Compose**

  ```shell
  sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  ```

  This command installs the following packages:
  - `docker-ce`: Docker Community Edition
  - `docker-ce-cli`: Docker command-line interface
  - `containerd.io`: Containerd runtime
  - `docker-buildx-plugin`: Docker buildx plugin for advanced build capabilities
  - `docker-compose-plugin`: Docker Compose plugin for defining and running multi-container applications

### 3. Start the Docker Service

Start the Docker service to initialize Docker and enable it to start automatically on system boot.

```shell
sudo systemctl start docker
```

- **Enable Docker to Start at Boot**

  Optionally, you can enable Docker to start automatically on boot by running:

  ```shell
  sudo systemctl enable docker
  ```

### 4. Verify Docker Installation

To confirm that Docker is installed correctly and the service is running, execute a test container.

```shell
sudo docker run hello-world
```

This command runs the `hello-world` Docker image, which outputs a message indicating that Docker is working correctly. The expected output should include a message like:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```
