## Problem Statement

The Nautilus DevOps team is setting up microservices on the Kubernetes platform. They have already established a Kubernetes cluster and now need to set up namespaces and deployments. Here are the specific requirements:

1. Create a namespace named *dev*.
2. Deploy a Pod within this namespace named *dev-nginx-pod* using the *nginx* image with the latest tag (specify as *nginx:latest*).

## Solution

### 1. Create the Namespace

First, create the namespace where the Pod will be deployed.

```bash
kubectl create namespace dev
```

**Output:**

```
namespace/dev created
```

### 2. Create the Pod in the Namespace

Next, create the Pod within the *dev* namespace. You can do this using either the `kubectl run` command or a YAML file. Below are both methods:

#### Using `kubectl run` Command

```bash
kubectl run dev-nginx-pod --image=nginx:latest -n dev
```

**Output:**

```
pod/dev-nginx-pod created
```

#### Using a YAML File

Alternatively, you can define the Pod in a YAML file. Create a file named `dev-nginx-pod.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dev-nginx-pod
  namespace: dev
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
```

Apply the YAML file using:

```bash
kubectl apply -f dev-nginx-pod.yaml
```

**Output:**

```
pod/dev-nginx-pod created
```

### Verification

1. **Check the Namespace:**

   To ensure the namespace has been created, run:

   ```bash
   kubectl get namespaces
   ```

   **Expected Output:**

   ```
   NAME              STATUS   AGE
   default           Active   <age>
   dev               Active   <age>
   kube-node-lease   Active   <age>
   kube-public       Active   <age>
   kube-system       Active   <age>
   ```

2. **Verify the Pod:**

   To check if the Pod has been created and is running in the *dev* namespace:

   ```bash
   kubectl get pods -n dev
   ```

   **Expected Output:**

   ```
   NAME             READY   STATUS    RESTARTS   AGE
   dev-nginx-pod    1/1     Running   0          <age>
   ```

By following these steps, you will have successfully created a namespace and deployed a Pod in Kubernetes.
