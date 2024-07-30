# **ProblemStatement**

The Nautilus development team has completed development of one of the node applications, which they are planning to deploy on a Kubernetes cluster. They recently had a meeting with the DevOps team to share their requirements. Based on that, the DevOps team has listed out the exact requirements to deploy the app. Find below more details:

- Create a deployment using gcr.io/kodekloud/centos-ssh-enabled:node image, replica count must be 2.

- Create a service to expose this app, the service type must be NodePort, targetPort must be 8080 and nodePort should be 30012.

- Make sure all the pods are in Running state after the deployment.

- You can check the application by clicking on NodeApp button on top bar.

# **Solution:**

We need to create a deployment based on given requirements. Below is the yaml script for the deplaoyment

```
kubectl create deployment kodekloud --image=gcr.io/kodekloud/centos-ssh-enabled:node --replicas=2 --dry-run=client -o yaml
```

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

A service need to be created inorder to connect the containers to outside traffic. A nodeport service has to be created.

```

kubectl create service nodeport --tcp=8080:8080 --node-port=30012 kodekloud-svc --dry-run=client -o yaml

```

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
