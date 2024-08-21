## Problem Statement

 We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it

- Create a pod named *volume-share-datacenter*

- For the first container, use image *debian* with latest tag only and remember to mention the tag i.e *debian:latest*, container should be named as *volume-container-datacenter-1*, and run a *sleep* command for it so that it remains in running state. Volume *volume-share* should be mounted at path */tmp/media*

- For the second container, use image *debian* with the latest tag only and remember to mention the tag i.e *debian:latest*, container should be named as *volume-container-datacenter-2*, and again run a *sleep* command for it so that it remains in running state. Volume *volume-share* should be mounted at path */tmp/games*

- Volume name should be *volume-share* of type *emptyDir*

- After creating the pod, exec into the first container i.e *volume-container-datacenter-1*, and just for testing create a file *media.txt* with any content under the mounted path of first container i.e */tmp/media*

- The file *media.txt* should be present under the mounted path */tmp/games* on the second container *volume-container-datacenter-2* as well, since they are using a shared volume

To achieve the goal of sharing a volume between two containers within a Kubernetes pod, follow these steps to configure and verify the shared volume. Here’s how you can set up and test this scenario:

### Solution

#### 1. **Create the Pod Configuration**

We will create a pod named `volume-share-datacenter` with two containers:

- **Container 1** (`volume-container-datacenter-1`): Uses the `debian:latest` image and mounts the shared volume at `/tmp/media`.
- **Container 2** (`volume-container-datacenter-2`): Also uses the `debian:latest` image and mounts the same shared volume at `/tmp/games`.

The shared volume will be of type `emptyDir`, which provides a temporary storage that is shared between the containers in the pod.

Save the following YAML configuration to a file named `volume-share-pod.yaml`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-datacenter
spec:
  containers:
  - name: volume-container-datacenter-1
    image: debian:latest
    command:
    - "/bin/sh"
    - "-c"
    - "sleep 3600"  # Keeps the container running
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/media

  - name: volume-container-datacenter-2
    image: debian:latest
    command:
    - "/bin/sh"
    - "-c"
    - "sleep 3600"  # Keeps the container running
    volumeMounts:
    - name: volume-share
      mountPath: /tmp/games

  volumes:
  - name: volume-share
    emptyDir: {}
```

Apply the Pod configuration:

```bash
kubectl apply -f volume-share-pod.yaml
```

Check the status of the pod:

```bash
kubectl get pods
```

You should see the pod `volume-share-datacenter` with the status `Running`.

#### 2. **Create a File in the Shared Volume**

Next, exec into the first container to create a file in the shared volume.

```bash
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-1 -- /bin/bash
```

Inside the container, navigate to the mounted path and create a file:

```bash
cd /tmp/media
echo "This is a test file." > media.txt
```

#### 3. **Verify File Presence in the Second Container**

Now, exec into the second container and check if the file is visible in the shared volume:

```bash
kubectl exec -it volume-share-datacenter -c volume-container-datacenter-2 -- /bin/bash
```

Navigate to the mounted path and list the files:

```bash
cd /tmp/games
ls
```

You should see `media.txt` listed, confirming that the file created in the first container is visible in the second container as well.

### Summary

1. **Pod Configuration**: A pod `volume-share-datacenter` is created with two containers, both using the `debian:latest` image. A shared volume named `volume-share` is mounted at different paths in each container.
2. **Volume Sharing**: The `emptyDir` volume type is used to create a temporary shared storage.
3. **Testing**: After deploying the pod, you create a file in the shared volume from one container and verify its presence in the other container to ensure proper volume sharing.

This setup confirms that the shared volume between containers works as expected, allowing data to be exchanged between the containers in the same pod.
