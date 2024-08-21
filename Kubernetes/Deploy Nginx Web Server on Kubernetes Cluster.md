## Problem Statement

Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:

- Create a deployment using *nginx* image with latest tag only and remember to mention the tag i.e *nginx:latest*. Name it as *nginx-deployment*. The container should be named as *nginx-container*, also make sure replica counts are *3*.

- Create a NodePort type service named *nginx-service*. The nodePort should be *30011*.

## Solution

## 1. Create the Deployment

The Deployment will use the nginx:latest image, with 3 replicas for high availability.

File: `nginx-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  
  labels:
    app: nginx-deployment
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-deployment
  strategy: {}
  template:
    metadata:
      
      labels:
        app: nginx-deployment
    spec:
      containers:
      - image: nginx:latest
        name: nginx-container
        resources: {}
status: {}
```
