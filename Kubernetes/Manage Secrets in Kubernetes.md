## Problem Statement:

 The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements

-  We already have a secret key file *official.txt* under */opt* location on jump host. Create a generic secret named *official*, it should contain the password/license-number present in *official.txt* file

-  Also create a pod named *secret-devops*

-  Configure pod's spec as container name should be *secret-container-devops*, image should be centos preferably with latest tag (remember to mention the tag with image). Use *sleep* command for container so that it remains in running state. Consume the created secret and mount it under */opt/demo* within the container

-  To verify you can exec into the container *secret-container-devops*, to check the secret key under the mounted path */opt/demo*. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience

Here’s a detailed breakdown and solution to securely store and access secrets using Kubernetes.

## Problem Breakdown:

1. **Create a Secret**:
   - We have a file `/opt/official.txt` on a jump host that contains a password/license number. This file should be used to create a Kubernetes secret.

2. **Create a Pod**:
   - We need to create a pod named `secret-devops` with a CentOS container.
   - The container should mount the created secret at `/opt/demo` to verify that the secret is correctly mounted and accessible.

### Solution:

#### Step 1: Create the Kubernetes Secret

To create a Kubernetes secret from the file `/opt/official.txt`:

```bash
kubectl create secret generic official --from-file=password=/opt/official.txt
```

This command creates a secret named `official` with the data from the file. The key in the secret will be `password`, and its value will be the content of `official.txt`.

#### Step 2: Create the Pod YAML Configuration

Create a YAML file named `pod.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops
spec:
  containers:
  - name: secret-container-devops
    image: centos:latest
    command:
      - 'sleep'
      - '3600'
    volumeMounts:
    - name: secret-volume
      mountPath: /opt/demo
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: official
```

#### Explanation

- **`apiVersion: v1`**: Specifies the API version.
- **`kind: Pod`**: Specifies the resource type.
- **`metadata`**: Contains metadata about the pod such as its name.
- **`spec`**: Contains the specifications for the pod, including:
  - **`containers`**:
    - **`name`**: The name of the container.
    - **`image`**: The container image (CentOS in this case).
    - **`command`**: The command to run (in this case, `sleep 3600` keeps the container running for 3600 seconds).
    - **`volumeMounts`**: Mounts the secret volume to `/opt/demo`.
  - **`volumes`**:
    - **`name`**: The name of the volume.
    - **`secret`**: Specifies that this volume is populated from a secret, using the secret name `official`.

#### Step 3: Apply the Pod YAML Configuration

Apply the YAML configuration to create the pod:

```bash
kubectl apply -f pod.yaml
```

Verify that the pod is running:

```bash
kubectl get pods
```

#### Step 4: Verify Secret Mount

To verify that the secret is mounted correctly, exec into the pod and check the mounted path:

```bash
kubectl exec -it secret-devops -- sh
```

Within the container:

```sh
cat /opt/demo/password
```

You should see the content of `official.txt`, e.g., `5ecur3`.

### Summary

- **Secret Creation**: Created a Kubernetes secret from a file.
- **Pod Configuration**: Created a pod with CentOS that mounts the secret at a specified path.
- **Verification**: Checked that the secret is accessible inside the container.

This process ensures that sensitive information is securely stored and accessible only where needed.

