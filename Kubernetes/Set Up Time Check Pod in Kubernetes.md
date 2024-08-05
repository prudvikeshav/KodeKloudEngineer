## Problem Statement

The Nautilus DevOps team needs to create a time-check Pod in a specific Kubernetes namespace for logging purposes. Initially, this setup is for testing but might be integrated into an existing cluster later. Here’s what is required:

- Create a Pod named *time-check* in the *devops* namespace.
- The Pod should contain a container named *time-check*, using the *busybox* image with the latest tag (specify as `busybox:latest`).
- Create a ConfigMap named *time-config* with the data `TIME_FREQ=12` in the same namespace.
- Configure the *time-check* container to execute the command: `while true; do date; sleep $TIME_FREQ; done`. Ensure the result is written to `/opt/devops/time/time-check.log`.
- Add an environmental variable *TIME_FREQ* in the container, fetching its value from the ConfigMap *TIME_FREQ* key.
- Create a volume *log-volume* and mount it at `/opt/devops/time` within the container.

## Solution

### 1. Create the Namespace

First, create the namespace where the Pod and ConfigMap will be deployed.

```bash
kubectl create namespace devops
```

**Output:**

```
namespace/devops created
```

### 2. Create the ConfigMap

Create a ConfigMap named *time-config* in the *devops* namespace with the specified data.

```bash
kubectl create configmap time-config --from-literal=TIME_FREQ=12 --namespace=devops
```

**Output:**

```
configmap/time-config created
```

### 3. Create the Pod YAML Configuration

Create a YAML file named `time-check-pod.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: devops
spec:
  volumes:
  - name: log-volume
    emptyDir: {}
  containers:
  - name: time-check
    image: busybox:latest
    volumeMounts:
    - name: log-volume
      mountPath: /opt/devops/time/
    env:
    - name: TIME_FREQ
      valueFrom:
        configMapKeyRef:
          name: time-config
          key: TIME_FREQ
    command: ["/bin/sh", "-c"]
    args:
    - "while true; do date; sleep $TIME_FREQ; done > /opt/devops/time/time-check.log"
```

### 4. Apply the Pod Configuration

Apply the Pod configuration to the Kubernetes cluster.

```bash
kubectl apply -f time-check-pod.yaml
```

**Output:**

```
pod/time-check created
```

### Verification

1. **Verify the Pod Creation:**

   ```bash
   kubectl get pods --namespace=devops
   ```

   **Expected Output:**

   ```
   NAME         READY   STATUS    RESTARTS   AGE
   time-check   1/1     Running   0          <age>
   ```

2. **Check the Logs:**

   To verify that the command is running correctly and logs are being generated, you can use:

   ```bash
   kubectl exec -it time-check --namespace=devops -- cat /opt/devops/time/time-check.log
   ```

This solution sets up a Pod with the specified requirements, using ConfigMaps for environment variables, and ensures logging to a mounted volume.
