## Problem Statement

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

- The deployment name is *python-deployment-nautilus*, its using *poroko/flask-demo-app* image. The deployment and service of this app is already deployed.

- nodePort should be *32345* and targetPort should be python flask app's default port.

To resolve the issue with the Python application deployment on the Kubernetes cluster, follow these steps to fix the configuration problems:

## **Solution**

### **1. Check Deployment Details**

First, let's review the deployment details to identify any issues:

```bash
kubectl describe deployments.apps python-deployment-datacenter
```

**Issues Observed:**

- **Image Name**: The image name is incorrectly specified.
- **Replica Availability**: The deployment shows `0 available` pods, which indicates there might be an issue with the pods not being ready.

### **2. Correct the Deployment Configuration**

If the image name is incorrect, you need to update it. Based on the provided details, it looks like you need to correct the image name in the deployment configuration. The correct image name should be `poroko/flask-demo-app`.

To edit the deployment:

```bash
kubectl edit deployments.apps python-deployment-datacenter
```

Change the image name in the container specification to `poroko/flask-demo-app` and save the changes.

**Example Correction:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-deployment-datacenter
spec:
  replicas: 1
  selector:
    matchLabels:
      app: python_app
  template:
    metadata:
      labels:
        app: python_app
    spec:
      containers:
      - name: python-container-datacenter
        image: poroko/flask-demo-app  # Corrected image name
        ports:
        - containerPort: 5000         # Flask default port
```

### **3. Update the Service Configuration**

Next, ensure that the service is correctly configured to match the container's port and the specified NodePort. You observed that the container port is `5000` and the NodePort should be `32345`.

To update the service configuration:

```bash
kubectl edit svc python-service-datacenter
```

Update the `port` and `nodePort` to reflect these values.

**Example Correction:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: python-service-datacenter
spec:
  type: NodePort
  selector:
    app: python_app
  ports:
  - protocol: TCP
    port: 5000        # Container port
    targetPort: 5000  # Forward to container port
    nodePort: 32345   # NodePort
```

### **4. Verify the Changes**

After making these updates, ensure that the changes have been applied and that the deployment is running correctly:

1. **Check Deployment Status:**

    ```bash
    kubectl get deployments.apps
    ```

    Confirm that the deployment has 1 replica available.

2. **Check Pods Status:**

    ```bash
    kubectl get pods
    ```

    Ensure that the pod is running and ready.

3. **Check Services Status:**

    ```bash
    kubectl get svc
    ```

    Confirm that the `python-service-datacenter` service is correctly configured with the NodePort `32345` and the correct target port.

### **5. Access the Application**

Once you confirm that the deployment and service are correctly configured, access the application using the NodePort. Replace `<NodeIP>` with the IP address of your node:

```bash
http://<NodeIP>:32345
```

Check if the application is accessible and functioning as expected.

### **6. Verify Application Logs**

If the application is still not working, check the logs of the pod to diagnose any further issues:

```bash
kubectl logs <pod-name>
```

Replace `<pod-name>` with the name of the running pod.

By following these steps, you should be able to correct the deployment and service configuration, and make the Python application accessible on the specified NodePort.
