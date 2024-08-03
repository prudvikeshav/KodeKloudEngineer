## Problem Statement

 There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below

-  Create a namespace named as *httpd-namespace-nautilus*

-  Create a deployment named as *httpd-deployment-nautilus* under newly created namespace. For the deployment use *httpd* image with latest tag only and remember to mention the tag i.e *httpd:latest*, and make sure replica counts are *2*

-  Create a service named as *httpd-service-nautilus* under same namespace to expose the *deployment*, *nodePort* should be *30004*

## Solution:

 To create a namespace.
```bash
kubectl create namespace httpd-namespace-nautilus
namespace/httpd-namespace-nautilus created
```
 Created a Deployment with the given details.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: httpd-deployment-nautilus
  name: httpd-deployment-nautilus
  namespace: httpd-namespace-nautilus
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd-deployment-nautilus
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: httpd-deployment-nautilus
    spec:
      containers:
      - image: httpd:latest
        name: httpd
        resources: {}
status: {}
```

 Apply the deployment File

```
kubectl apply -f deployment.yaml
deployment.apps/httpd-deployment-nautilus created
```
 Create a service with given details.

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: httpd-service-nautilus
  name: httpd-service-nautilus
  namespace: httpd-namespace-nautilus
spec:
  ports:
  - name: httpd-service
    port: 80
    protocol: TCP
    targetPort: 80
    nodePort: 30004
  selector:
    app: httpd-deployment-nautilus
  type: NodePort
status:
  loadBalancer: {}
```

 Apply the Service file.

```bash
kubectl apply -f service.yaml
```

```
service/httpd-service-nautilus created
```

 To check the service created in our namespace.

```bash
kubectl get svc -n httpd-namespace-nautilus
```

```
NAME                     TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
httpd-service-nautilus   NodePort   10.96.14.204   <none>        80:30004/TCP   10s
```

 To check the deployments created.

```bash
kubectl get deployments.apps -n httpd-namespace-nautilus
```

```
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
httpd-deployment-nautilus   2/2     2            2           6m37s
```
