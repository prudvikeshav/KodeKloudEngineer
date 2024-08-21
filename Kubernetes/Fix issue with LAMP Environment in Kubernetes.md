## **Problem Statement**

One of the DevOps team member was trying to install a WordPress website on a LAMP stack which is essentially deployed on Kubernetes cluster. It was working well and we could see the installation page a few hours ago. However something is messed up with the stack now due to a website went down. Please look into the issue and fix it

 FYI, deployment name is *lamp-wp* and its using a service named *lamp-service*. The Apache is using http default port and nodeport is *30008*. From the application logs, it has been identified that the application is facing some issues while connecting to the database in addition to other issues. Additionally, there are some environment variables associated with the pods like *MYSQL_ROOT_PASSWORD, MYSQL_DATABASE,  MYSQL_USER, MYSQL_PASSWORD, MYSQL_HOST

## **Solution**

### **1. Check Deployment Status**

First, ensure that the `lamp-wp` deployment is running properly:

```bash
kubectl get deployments.apps
```

Expected output:

```
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
lamp-wp   1/1     1            1           14m
```

### **2. Check Service Configuration**

Verify the configuration of the services to ensure that they are correctly set up:

```bash
kubectl get svc
```

Expected output:

```
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)            AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP            59m
lamp-service    NodePort    10.96.14.0     <none>        8080:30008/TCP     15m
mysql-service   ClusterIP   10.96.89.233   <none>        3306/TCP           15m
```

### **3. Fix the Port Configuration of `lamp-service`**

The `lamp-service` should be exposed on port `80` (default HTTP port) rather than `8080`. Update the service configuration:

```bash
kubectl edit svc lamp-service
```

Change the `PORT(S)` section from `8080:30008/TCP` to `80:30008/TCP`. Save the changes.

Verify the update:

```bash
kubectl get svc
```

Expected output:

```
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)            AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP            59m
lamp-service    NodePort    10.96.14.0     <none>        80:30008/TCP       15m
mysql-service   ClusterIP   10.96.89.233   <none>        3306/TCP           15m
```

### **4. Check Pod Status**

Verify that the pods are running:

```bash
kubectl get pods
```

Expected output:

```
NAME                       READY   STATUS    RESTARTS   AGE
lamp-wp-56c7c454fc-556v2   2/2     Running   0          16m
```

### **5. View Pod Logs**

Inspect the logs of the `lamp-wp` pod to diagnose issues, especially related to the database connection:

```bash
kubectl logs lamp-wp-56c7c454fc-556v2
```

Look for errors or warnings related to database connectivity, environment variables, or other issues.

### **6. Check Environment Variables**

Ensure that the environment variables required for connecting to the MySQL database are correctly set in the `lamp-wp` deployment. These variables should be configured in the deployment manifest. Verify this by describing the deployment:

```bash
kubectl describe deployment lamp-wp
```

Ensure the following environment variables are correctly configured in the pod's container specification:

- `MYSQL_ROOT_PASSWORD`
- `MYSQL_DATABASE`
- `MYSQL_USER`
- `MYSQL_PASSWORD`
- `MYSQL_HOST`

If they are not set or incorrect, update the deployment configuration with the correct values:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lamp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lamp-wp
  template:
    metadata:
      labels:
        app: lamp-wp
    spec:
      containers:
      - name: lamp-container
        image: your-lamp-image
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: your_root_password
        - name: MYSQL_DATABASE
          value: your_database_name
        - name: MYSQL_USER
          value: your_user
        - name: MYSQL_PASSWORD
          value: your_password
        - name: MYSQL_HOST
          value: mysql-service
```

Update the deployment if necessary:

```bash
kubectl apply -f your-deployment.yaml
```

### **7. Verify Fixes**

Once you have made the necessary changes, verify that the issues are resolved:

1. **Check Pods**: Ensure all pods are running correctly.
2. **Access the Application**: Visit the WordPress site through the NodePort service to verify functionality.
3. **Review Logs**: Ensure there are no errors related to database connectivity or other critical issues.

By following these steps, you should be able to resolve the connectivity issues and ensure the WordPress application is functioning as expected.
