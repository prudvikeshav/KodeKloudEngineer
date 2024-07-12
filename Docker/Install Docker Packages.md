
### <span style="color:red"> Problem Description:</span>

#### *The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:*

#### *Install docker-ce and docker compose packages on App Server 2.*

#### *Initiate the docker service.*

### Solution

#### *Install yum-utils*

```shell
sudo yum install -y yum-utils
```

#### *Install docker packages*

```shell
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### *Start the docker service*

```shell
 sudo systemctl start docker
 ```

#### *Run a container*

```shell
sudo docker run hello-world
```
