 Problem Statement:

A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:

- Create a namespace named tomcat-namespace-nautilus.

- Create a deployment for tomcat app which should be named as tomcat-deployment-nautilus under the same namespace you created. Replica count should be 1, the container should be named as tomcat-container-nautilus, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.

- Create a service for tomcat app which should be named as tomcat-service-nautilus under the same namespace you created. Service type should be NodePort and nodePort should be 32227.

 Solution:

Here's a detailed solution for setting up a Tomcat application in a Kubernetes cluster, following the provided requirements:

## **Solution**

### **1. Create the Namespace**

First, create a namespace to organize the resources:

```bash
kubectl create namespace tomcat-namespace-nautilus
```

### **2. Create the Deployment**

Next, create a deployment named `tomcat-deployment-nautilus` within the `tomcat-namespace-nautilus` namespace. This deployment will manage a single replica of a container running Tomcat.

Save the following YAML configuration to a file named `tomcat-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-nautilus
  namespace: tomcat-namespace-nautilus
  labels:
    app: tomcat-deployment-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-deployment-nautilus
  template:
    metadata:
      labels:
        app: tomcat-deployment-nautilus
    spec:
      containers:
      - name: tomcat-container-nautilus
        image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
        ports:
        - containerPort: 8080
        resources: {}
```

Apply this configuration:

```bash
kubectl apply -f tomcat-deployment.yaml
```

### **3. Create the Service**

Create a service named `tomcat-service-nautilus` to expose the Tomcat application. This service will use the NodePort type and will expose the application on port 32227.

Save the following YAML configuration to a file named `tomcat-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-nautilus
  namespace: tomcat-namespace-nautilus
  labels:
    app: tomcat-deployment-nautilus
spec:
  ports:
  - name: tomcat-service-nautilus
    port: 8080
    targetPort: 8080
    nodePort: 32227
    protocol: TCP
  selector:
    app: tomcat-deployment-nautilus
  type: NodePort
```

Apply this configuration:

```bash
kubectl apply -f tomcat-service.yaml
```

### **4. Verify the Deployment and Service**

To ensure that everything is set up correctly, check the status of the deployment and service:

**Check Deployment:**

```bash
kubectl get deployments -n tomcat-namespace-nautilus
```

You should see `tomcat-deployment-nautilus` listed with 1 replica.

**Check Pods:**

```bash
kubectl get pods -n tomcat-namespace-nautilus
```

You should see a Pod created by the deployment with a status of `Running`.

**Check Service:**

```bash
kubectl get services -n tomcat-namespace-nautilus
```

You should see `tomcat-service-nautilus` listed with port `32227`.

### **Summary**

- **Namespace**: Created `tomcat-namespace-nautilus`.
- **Deployment**: Deployed `tomcat-deployment-nautilus` with 1 replica, using the `gcr.io/kodekloud/centos-ssh-enabled:tomcat` image, exposing port `8080`.
- **Service**: Created `tomcat-service-nautilus` to expose the deployment with a NodePort of `32227`.

This setup ensures that your Tomcat application is correctly deployed and accessible via the specified NodePort.
