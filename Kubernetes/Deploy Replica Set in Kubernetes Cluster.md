## Problem Statement

 The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below

 Create a ReplicaSet using *nginx* image with latest tag (ensure to specify as nginx:latest) and name it *nginx-replicaset*

 Apply labels: *app* as *nginx_app*, *type* as *front-end*

 Name the container *nginx-container*. Ensure the replica count is *4*

## Solution

### 1. Create the ReplicaSet YAML File

The following YAML configuration defines a ReplicaSet using the `nginx:latest` image, with a replica count of 4. It also applies the required labels and names the container `nginx-container`.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    type: front-end
    app: nginx_app
spec:

  replicas: 4
  selector:
    matchLabels:
      type: front-end
      app: nginx_app
  template:
    metadata:
      labels:
        type: front-end
        app: nginx_app
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
```

### 2. Apply the ReplicaSet Configuration

Save the YAML configuration to a file named `nginx-replicaset.yaml`, then apply it with the following command:

```bash
kubectl apply -f nginx-replicaset.yaml
```

### 3. Verify the ReplicaSet

After applying the configuration, check that the ReplicaSet is created and running as expected:

```bash
kubectl get replicasets.apps 
```

Expected Output:

```
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   4         4         0       8s
```

### 4. Verify Pods

To ensure that the Pods are correctly created and running, you can list the Pods and check their status:

```bash
kubectl get pods
```

You should see four Pods with names prefixed by nginx-replicaset (or similar), depending on the naming convention used by Kubernetes.

### Summary

This solution sets up a ReplicaSet with four replicas of an nginx container, using the nginx:latest image. The ReplicaSet is labeled as nginx_app and front-end, with the container named nginx-container. You verify its creation and status using kubectl commands to ensure it is running properly.
