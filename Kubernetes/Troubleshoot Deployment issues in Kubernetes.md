## Problem Statement

The Nautilus DevOps team is facing issues with their Redis application deployment on Kubernetes. The pods are not running due to errors related to the container image and ConfigMap.

## Solution

### 1. Identify the Issues

**Pod Status:**

```bash
kubectl get pods
```

**Output:**

```
NAME                                READY   STATUS              RESTARTS   AGE
redis-deployment-54cdf4f76d-lvj2m   0/1     ContainerCreating   0          52s
```

**Deployment Description:**

```bash
kubectl describe deployments.apps redis-deployment
```

**Output:**

```
Name:                   redis-deployment
Namespace:              default
CreationTimestamp:      Tue, 30 Jul 2024 04:46:52 +0000
Labels:                 app=redis
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=redis
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=redis
  Containers:
   redis-container:
    Image:      redis:alpin  # Error: Typo in image name
    Port:       6379/TCP
    Host Port:  0/TCP
    Requests:
      cpu:        300m
    Environment:  <none>
    Mounts:
      /redis-master from config (rw)
      /redis-master-data from data (rw)
  Volumes:
   data:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
   config:
    Type:          ConfigMap (a volume populated by a ConfigMap)
    Name:          redis-conig  # Error: Typo in ConfigMap name
    Optional:      false
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   redis-deployment-54cdf4f76d (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  98s   deployment-controller  Scaled up replica set redis-deployment-54cdf4f76d to 1
  Warning FailedMount       90s   kubelet, node1          MountVolume.SetUp failed for volume "config" : configmap "redis-conig" not found
  Warning FailedMount       69s   kubelet, node1          Unable to attach or mount volumes: unmounted volumes=[config], unattached volumes=[], failed to process volumes=[]: timed out waiting for the condition
```

**Issues Identified:**

1. **Image Name Typo:** The image specified is `redis:alpin` instead of `redis:alpine`.
2. **ConfigMap Name Typo:** The ConfigMap specified is `redis-conig` instead of `redis-config`.

### 2. Fix the Deployment

**Update the Deployment:**

To fix the issues, you need to update the deployment configuration. First, create a corrected YAML file or edit the deployment directly.

**Edit the Deployment:**

```bash
kubectl edit deployment redis-deployment
```

**Update the following fields in the editor:**

```yaml
spec:
  containers:
  - name: redis-container
    image: redis:alpine  # Corrected image name
  volumes:
  - name: config
    configMap:
      name: redis-config  # Corrected ConfigMap name
```

**Apply the Updated Configuration:**

Alternatively, you can apply a corrected YAML configuration file. Create or edit the file named `redis-deployment-corrected.yaml` with the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        ports:
        - containerPort: 6379
        volumeMounts:
        - name: config
          mountPath: /redis-master
        - name: data
          mountPath: /redis-master-data
      volumes:
      - name: config
        configMap:
          name: redis-config
      - name: data
        emptyDir: {}
```

**Apply the Updated YAML:**

```bash
kubectl apply -f redis-deployment-corrected.yaml
```

### 3. Verify the Fix

**Check Pods Status:**

```bash
kubectl get pods
```

**Verify Pod Details:**

```bash
kubectl describe pods redis-deployment-54cdf4f76d-<pod-suffix>
```

**Check Events:**

```bash
kubectl get events
```

By following these steps, the issues with the Redis deployment should be resolved, and the pods should be running as expected.
