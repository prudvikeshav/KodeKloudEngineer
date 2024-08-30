# Problem Statement

---
 There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below

- Create a namespace named as *httpd-namespace-nautilus*

- Create a deployment named as *httpd-deployment-nautilus* under newly created namespace. For the deployment use *httpd* image with latest tag only and remember to mention the tag i.e *httpd:latest*, and make sure replica counts are *2*

- Create a service named as *httpd-service-nautilus* under same namespace to expose the *deployment*, *nodePort* should be *30004*

## Solution

### 1. Create a NameSpace

 To create the namespace where the deployment and service will reside:

```bash
kubectl create namespace httpd-namespace-nautilus
```

This command will create a new namespace called httpd-namespace-nautilus.

```plain
namespace/httpd-namespace-nautilus created
```

### 2. Create a Deployment

Create a Kubernetes Deployment by saving the following YAML configuration to a file named `deployment.yaml`:

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

Apply the deployment configuration:

```plain
kubectl apply -f deployment.yaml
deployment.apps/httpd-deployment-nautilus created
```

### 3. Create Service

Save the following YAML configuration to a file named `service.yaml`:

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

Details:

- apiVersion: v1: The API version for the Service resource.
- kind: Service: Specifies the resource type.
- metadata: Metadata about the Service including name and namespace.
- spec: Specification of the Service.
- type: NodePort: Exposes the Service on a static port on each Node.
- selector: Selects the pods that this Service will route traffic to.
- ports: Defines the ports configuration.
- port: Port the service will expose.
- targetPort: Port on the container to which the traffic will be directed.
- nodePort: Port on the Node where the Service is exposed.

 Apply the Service file.

```bash
kubectl apply -f service.yaml
```

```plain
service/httpd-service-nautilus created
```

 To verify the Service and Deployment are correctly set up, use the following commands:

Check Services:

```bash
kubectl get svc -n httpd-namespace-nautilus
```

Expected Output:

```plain
NAME                     TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
httpd-service-nautilus   NodePort   10.96.14.204   <none>        80:30004/TCP   10s
```

Check Deployment:

```bash
kubectl get deployments.apps -n httpd-namespace-nautilus
```

Expected Output:

```plain
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
httpd-deployment-nautilus   2/2     2            2           6m37s
```
