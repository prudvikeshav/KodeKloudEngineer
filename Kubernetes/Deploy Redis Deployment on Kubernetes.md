## Problem Statement

The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:

- Create a redis deployment with following parameters:

- Create a config map called *my-redis-config* having maxmemory *2mb* in *redis-config*.

- Name of the deployment should be *redis-deployment*, it should use
*redis:alpine* image and container name should be *redis-container*. Also make sure it has only *1* replica.

- The container should request for *1* CPU.

- Mount 2 volumes:

  - An Empty directory volume called data at path */redis-master-data*.

  - A configmap volume called *redis-config* at path */redis-master*.

  - The container should expose the port *6379*.

Finally, redis-deployment should be in an up and running state.

## Solution

1. Create a ConfigMap
First, create a ConfigMap named `my-redis-config` with the Redis configuration. This ConfigMap will contain the setting for `maxmemory`.

```bash
kubectl create configmap my-redis-config --from-literal=maxmemory=2mb
```

Expected output:

```
configmap/my-redis-config created
```

### 2. Create the Redis Deployment

Create a Deployment named `redis-deployment` using the `redis:alpine` image. The Deployment should have 1 replica, request for 1 CPU, and mount two volumes: an empty directory and a ConfigMap.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis-deployment
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-deployment
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: redis-deployment
    spec:
      containers:
      - image: redis:alpine
        name: redis-container
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: 1
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
      volumes:
      - name: redis-config
        configMap: 
          name: my-redis-config
      - name: data
        emptyDir: {}
status: {}
```

### 3. Apply the YAML Configuration

Save the YAML configuration to a file named `redis-deployment.yaml`, then apply it with:

```bash
kubectl apply -f redis-deployment.yaml 
```

Expected Output:

```
deployment.apps/redis-deployment created
```

### 4. Verify Deployment

After applying the configuration, check that the Redis deployment is up and running:

```bash
kubectl get deployments
kubectl get pods
```

Make sure the redis-deployment is listed and that the associated pod is in the Running state.

### Summary

This solution sets up a Redis deployment with the required configuration and volumes. The deployment uses the redis:alpine image, requests 1 CPU, and mounts a ConfigMap and an empty directory as volumes.
