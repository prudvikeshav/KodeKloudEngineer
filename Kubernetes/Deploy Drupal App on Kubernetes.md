# **Problem statement:**
We need to deploy a Drupal application on Kubernetes cluster. The Nautilus application development team want to setup a fresh Drupal as they will do the installation on their own. Below you can find the requirements, they have shared with us.



- Configure a persistent volume drupal-mysql-pv with hostPath = /drupal-mysql-data (/drupal-mysql-data directory already exists on the worker Node i.e jump host), 5Gi of storage and ReadWriteOnce access mode.


- Configure one PersistentVolumeClaim named drupal-mysql-pvc with storage request of 3Gi and ReadWriteOnce access mode.

- Create a deployment drupal-mysql with 1 replica, use mysql:5.7 image. Mount the claimed PVC at /var/lib/mysql.


- Create a deployment drupal with 1 replica and use drupal:8.6 image.


- Create a NodePort type service which should be named as drupal-service and nodePort should be 30095.


- Create a service drupal-mysql-service to expose mysql deployment on port 3306.


- Set rest of the configration for deployments, services, secrets etc as per your preferences. At the end you should be able to access the Drupal installation page by clicking on App button.

# **Solution:**
Firstly we need to create secrets for the mysql container

```bash
kubectl create secret generic mysql-root-pass --from-literal=password=R00t
kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_pop --from-literal=password=BruCStnMT5
kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db5
kubectl create secret generic mysql-host --from-literal=host=mysql-service
```
A manifest file where all the deployments, services, persistant volume and persistant volumeclaim are manifested according to the requirements.
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: drupal-mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 3Gi
  storageClassName: slow

---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-mysql-pv
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /drupal-mysql-data
  storageClassName: slow
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal-mysql
spec:
  selector:
    matchLabels:
      app: drupal-mysql
  template:
    metadata:
      labels:
        app: drupal-mysql
    spec:
      containers:
      - name: drupal-mysql
        image: mysql:5.7
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
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: mysql-host
              key: host
        volumeMounts:
        - mountPath: "/var/lib/mysql"
          name: mysql
        ports:
        - containerPort: 3306
      volumes:
      - name: mysql
        persistentVolumeClaim:
          claimName: drupal-mysql-pvc
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal
spec:
  selector:
    matchLabels:
      app: drupal
  template:
    metadata:
      labels:
        app: drupal
    spec:
      containers:
      - name: drupal
        image: drupal:8.6
        ports:
        - containerPort: 80

---
kind: Service
apiVersion: v1
metadata:
  name: drupal-service
spec:
  selector:
    app: drupal
  type: NodePort
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30095
---

apiVersion: v1
kind: Service
metadata:
  name: drupal-mysql-service
spec:
  selector: 
    app: drupal-mysql
  type: ClusterIP
  ports:
  - port: 3306
    targetPort: 3306
```
```bash
kubectl apply -f drupal.yaml 
```
Output:

```
persistentvolumeclaim/drupal-mysql-pvc created
persistentvolume/drupal-mysql-pv created
deployment.apps/drupal-mysql created
deployment.apps/drupal created
service/drupal-service created
service/drupal-mysql-service created
```