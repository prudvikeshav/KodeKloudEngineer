# **Problem Statement:**

The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:

- 1) Create a namespace jenkins

- 2) Create a Service for jenkins deployment. Service name should be jenkins-service under jenkins namespace, type should be NodePort, nodePort should be 30008

- 3) Create a Jenkins Deployment under jenkins namespace, It should be name as jenkins-deployment , labels app should be jenkins , container name should be jenkins-container , use jenkins/jenkins image , containerPort should be 8080 and replicas count should be 1.

# **Solution:**

We need to create a namespace jenkins

```bash
kubectl create namespace jenkins
```

```
namespace/jenkins created
```

To create a deploymement with the above requirements

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  
  labels:
    app: jenkins
  name: jenkins-deployment
  namespace: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  strategy: {}
  template:
    metadata:
      
      labels:
        app: jenkins
    spec:
      containers:
      - image: jenkins/jenkins
        name: jenkins-container
        ports:
        - containerPort: 8080
        resources: {}
status: {}
```

To create a service with the requirements

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: jenkins-service
  name: jenkins-service
  namespace: jenkins
spec:
  ports:
  - name: jenkins-service
    nodePort: 30008
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: jenkins
  type: NodePort
status:
  loadBalancer: {}
```
