## Problem Statement

There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on the Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.

- Create a namespace *datacenter*. Create a deployment called *httpd-deploy* under this new namespace, It should have one container called *httpd*, use *httpd:2.4.28* image and *2* replicas. The deployment should use *RollingUpdate* strategy with *maxSurge=1*, and *maxUnavailable=2*. Also create a NodePort type service named *httpd-service* and expose the deployment on *nodePort: 30008*.

- Now upgrade the deployment to version *httpd:2.4.43* using a rolling update.

- Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

## Solution

### 1. Create the Namespace

First, create the namespace *datacenter*.

```bash
kubectl create namespace datacenter
```

Output:

```
namespace/datacenter created
```

### 2. Set the Namespace Context

Change the current context to the newly created namespace *datacenter*.

```bash
kubectl config set-context --current --namespace=datacenter
```

Output:

```
Context "kind-kodekloud" modified.
```

### 3. Create the Deployment

Create the deployment *httpd-deploy* with the specified requirements.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: httpd
  name: httpd-deploy
  namespace: datacenter
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - image: httpd:2.4.28
        name: httpd
        resources: {}
status: {}
```

Apply the deployment configuration:

```bash
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: httpd
  name: httpd-deploy
  namespace: datacenter
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - image: httpd:2.4.28
        name: httpd
        resources: {}
status: {}
EOF
```

### 4. Create the NodePort Service

Create the NodePort service *httpd-service* to expose the deployment.

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: httpd-service
  name: httpd-service
  namespace: datacenter
spec:
  ports:
  - name: httpd-service
    nodePort: 30008
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: httpd
  type: NodePort
status:
  loadBalancer: {}
```

Apply the service configuration:

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  labels:
    app: httpd-service
  name: httpd-service
  namespace: datacenter
spec:
  ports:
  - name: httpd-service
    nodePort: 30008
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: httpd
  type: NodePort
status:
  loadBalancer: {}
EOF
```

### 5. Upgrade the Deployment

Upgrade the deployment to use the *httpd:2.4.43* image using a rolling update.

```bash
kubectl set image deployment/httpd-deploy httpd=httpd:2.4.43
```

### 6. Verify the Update

Check the status of the deployment to ensure that all pods are updated.

```bash
kubectl rollout status deployment/httpd-deploy
```

### 7. Rollback the Deployment

Once the update is complete, roll back to the previous version.

```bash
kubectl rollout undo deployment/httpd-deploy
```

### 8. Verify the Rollback

Check the deployment status to ensure it has reverted to the original version.

```bash
kubectl get deployment httpd-deploy -o wide
```

This sequence of commands and configurations will create the required deployment and service, perform a rolling update to a new image version, and then roll back to the previous version as specified.
