# **Problem Statement:**

There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.

- Create a namespace datacenter. Create a deployment called httpd-deploy under this new namespace, It should have one container called httpd, use httpd:2.4.28 image and 2 replicas. The deployment should use RollingUpdate strategy with maxSurge=1, and maxUnavailable=2. Also create a NodePort type service named httpd-service and expose the deployment on nodePort: 30008.

- Now upgrade the deployment to version httpd:2.4.43 using a rolling update.

- Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

# **Solution:**

kubectl create namespace datacenter
namespace/datacenter created

kubectl config set-context --current --namespace=datacenter
Context "kind-kodekloud" modified.

kubectl create deployment httpd-deploy --namespace=datacenter --image=httpd:2.4.28 --replicas=2 --dry-run=client -o yaml

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


```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: httpd-service
  name: httpd-service
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
