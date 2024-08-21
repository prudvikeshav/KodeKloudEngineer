## Problem Statement

The Nautilus development team has completed development of one of the node applications, which they are planning to deploy on a Kubernetes cluster. They recently had a meeting with the DevOps team to share their requirements. Based on that, the DevOps team has listed out the exact requirements to deploy the app. Find below more details:

- Create a deployment using *gcr.io/kodekloud/centos-ssh-enabled:node* image, replica count must be *2*.

- Create a service to expose this app, the service type must be NodePort, targetPort must be *8080* and nodePort should be *30012*.

- Make sure all the pods are in Running state after the deployment.

- You can check the application by clicking on *NodeApp* button on top bar.

## Solution

### 1. Deployment YAML

This YAML defines the deployment for the application with the specified image and replica count.

We can create a deployment using command line based on given requirements.

```
kubectl create deployment kodekloud --image=gcr.io/kodekloud/centos-ssh-enabled:node --replicas=2 --dry-run=client -o yaml
```

 Below is the yaml script for the deployment
 File: `Deployment.yaml`

```deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: kodekloud
  name: kodekloud
spec:
  replicas: 2
  selector:
    matchLabels:
      app: kodekloud
  strategy: {}
  template:
    metadata:
      labels:
        app: kodekloud
    spec:
      containers:
      - image: gcr.io/kodekloud/centos-ssh-enabled:node
        name: centos-ssh-enabled
        resources: {}
status: {}
```

### 2. Service YAML

We can create a Service using a command line. Below is the command line:

```
kubectl create service nodeport --tcp=8080:8080 --node-port=30012 kodekloud-svc --dry-run=client -o yaml

```

This is the Yaml configuration for the Service it creates a NodePort service to expose the deployment'
File:  `service.yaml`

```service.yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: kodekloud-servcie
  name: kodekloud-servcie
spec:
  ports:
  - name: kodekloud-svc
    nodePort: 30012
    port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: kodekloud-servcie
  type: NodePort
status:
  loadBalancer: {}
```

### Accessing the Application

You can access the Node application using the NodePort on any of the cluster nodes:

```bash
http://<node-ip>:30012
```

Make sure to replace `<node-ip>` with the IP address of one of your cluster nodes.

By following these steps, you ensure that the Node application is deployed, accessible, and meeting the specified requirements.
