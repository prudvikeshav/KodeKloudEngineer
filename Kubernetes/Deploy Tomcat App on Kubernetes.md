 Problem Statement:

A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:

- Create a namespace named tomcat-namespace-nautilus.

- Create a deployment for tomcat app which should be named as tomcat-deployment-nautilus under the same namespace you created. Replica count should be 1, the container should be named as tomcat-container-nautilus, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.

- Create a service for tomcat app which should be named as tomcat-service-nautilus under the same namespace you created. Service type should be NodePort and nodePort should be 32227.

 Solution:

To create a namespace tomcat-namespace-nautilus

```
kubectl create namespace tomcat-namespace-nautilus
```

```
namespace/tomcat-namespace-nautilus created
```

We need to create a deployment with the given requirements

```
kubectl create deployment tomcat-deployment-nautilus --namespace=tomcat-namespace-nautilus --replicas=1 --image=gcr.io/kodekloud/centos-ssh-enabled:tomcat --port=8080 --dry-run=client -o yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: tomcat-deployment-nautilus
  name: tomcat-deployment-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-deployment-nautilus
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: tomcat-deployment-nautilus
    spec:
      containers:
      - image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
        name: tomcat-container-nautilus
        ports:
        - containerPort: 8080
        resources: {}
status: {}
```

We need to create a nodeport service for the deployment

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: tomcat-deployment-nautilus
  name: tomcat-service-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  ports:
  - name: tomcat-service-nautilus
    nodePort: 32227
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: tomcat-deployment-nautilus
  type: NodePort
status:
  loadBalancer: {}
```
