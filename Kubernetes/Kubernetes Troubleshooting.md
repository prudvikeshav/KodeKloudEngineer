## Problem Statement:

One of the Nautilus DevOps team members was working on to update an existing Kubernetes template. Somehow, he made some mistakes in the template and it is failing while applying. We need to fix this as soon as possible, so take a look into it and make sure you are able to apply it without any issues. Also, do not remove any component from the template like pods/deployments/volumes etc.


` */home/thor/mysql_deployment.yml* is the template that needs to be fixed.
## Solution:

The provided YAML configuration for the MySQL deployment looks well-structured but contains a few issues that need addressing. Here’s a detailed explanation of the corrections made:

### Corrected YAML Configuration

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: standard
  capacity:
    storage: 250Mi
  accessModes:
  - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: Retain
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
    app: mysql-app
spec:
  storageClassName: standard
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql-app
spec:
  type: NodePort
  ports:
  - targetPort: 3306
    port: 3306
    nodePort: 30011
  selector:
    app: mysql-app
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-app
spec:
  selector:
    matchLabels:
      app: mysql-app
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql-app
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim
```

### Issues Addressed

1. **Service Selector Labels**:
   - **Original**: `selector: app: mysql_app` and `tier: mysql`
   - **Correction**: Changed to `selector: app: mysql-app` to match the labels used in the Deployment.

2. **Deployment Selector Labels**:
   - **Original**: `matchLabels: app: mysql_app, tier: mysql`
   - **Correction**: Changed to `matchLabels: app: mysql-app` to match the labels used in the Service and the Deployment’s metadata.

3. **Deployment Labels**:
   - **Original**: `labels: app: mysql_app, tier: mysql`
   - **Correction**: Changed to `labels: app: mysql-app` to maintain consistency with the Service selector.

4. **Container Environment Variables**:
   - **Corrected**: Ensured the `valueFrom` references the correct `secretKeyRef` names and keys.

### Steps to Apply the Configuration

1. **Save the YAML File**:
   Ensure you have saved the corrected YAML configuration to `mysql_deployment.yml`.

2. **Apply the Configuration**:
   Use the `kubectl apply` command to apply the changes:

   ```bash
   kubectl apply -f mysql_deployment.yml
   ```

3. **Verify Resources**:
   Check that the resources have been created or updated as expected:

   ```bash
   kubectl get all
   ```

   This should show the PersistentVolume, PersistentVolumeClaim, Service, and Deployment with their respective statuses. Ensure that the Deployment is in a `Running` state and the pods are created without issues.
