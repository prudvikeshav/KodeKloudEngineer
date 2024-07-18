**Problem Statement**

#### The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below

#### Create a ReplicaSet using nginx image with latest tag (ensure to specify as nginx:latest) and name it nginx-replicaset

#### Apply labels: app as nginx_app, type as front-end

#### Name the container nginx-container. Ensure the replica count is 4

**Solution**

#### Create a replicaset with the given details

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

#### Check the replica set running or not

```bash
thor@jumphost ~$ kubectl get replicasets.apps 
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   4         4         0       8s
```
