## Problem Statement

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:

Create a pod named *httpd-pod* with a container named *httpd-container*. Use the *httpd* image with the latest tag (specify as httpd:latest). Set the following resource limits:

- Requests:
  - Memory: *15Mi*
  - CPU: *100m*

- Limits:
  - Memory: *20Mi*
  - CPU: *100m*

## Solution

### 1. Create the Pod Configuration

First, create a Pod YAML file with the specified requirements.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

### 2. Apply the Pod Configuration

Apply the Pod configuration to create the Pod in the Kubernetes cluster.

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      requests:
        memory: "15Mi"
        cpu: "100m"
      limits:
        memory: "20Mi"
        cpu: "100m"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
EOF
```

### 3. Verify the Pod

Check the Pods created to ensure that the Pod is properly set up and running.

```bash
kubectl get pods
```

Expected output:

```
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          <age>
```

By following these steps, you will create a Pod with specified resource requests and limits to manage resource utilization effectively. The Pod will be using the latest `httpd` image, and you can verify its status using the `kubectl get pods` command.
