## Problem Statement:

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:



- Create a PersistentVolume *mysql-pv*, its capacity should be 250Mi, set other parameters as per your preference.


- Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as*mysql-pv-claim* and request a *250Mi* of storage. Set other parameters as per your preference.


- Create a deployment named *mysql-deployment*, use any *mysql* image as per your preference. Mount the PersistentVolume at mount path */var/lib/mysql*.


- Create a NodePort type service named *mysql* and set nodePort to *30007*.


- Create a secret named *mysql-root-pass* having a key pair value, where key is *password* and its value is *YUIidhb667*, create another secret named *mysql-user-pass* having some key pair values, where frist key is *username* and its value is *kodekloud_joy*, second key is *password* and value is *dCV3szSGNA*, create one more secret named *mysql-db-url*, key name is *database* and value is *kodekloud_db2*


- Define some Environment variables within the container:


    - name: MYSQL_ROOT_PASSWORD, should pick value from secretKeyRef name: *mysql-root-pass* and key: *password*


    - name: MYSQL_DATABASE, should pick value from secretKeyRef name: *mysql-db-url* and key: *database*


    - name: MYSQL_USER, should pick value from secretKeyRef name: *mysql-user-pass* key key: *username*


    - name: MYSQL_PASSWORD, should pick value from secretKeyRef name: *mysql-user-pass* and key: *password*

## Solution:

We need to create secrets for the MySQL environment variables.

```bash
kubectl create secret generic mysql-root-pass --from-literal=password=YUIidhb667
kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_rin --from-literal=password=GyQkFRVNr3
kubectl create secret generic --from-literal=database=kodekloud_db10 mysql-db-url
```
Manifest file for deployment, Persistent volume, Persistentvolumeclaim and nodeport service.

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: mysql
  name: mysql
spec:
  ports:
  - name: mysql
    nodePort: 30007
    port: 3306
    protocol: TCP
    targetPort: 3306
  selector:
    app: mysql
  type: NodePort
status:
  loadBalancer: {}

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 250Mi
  storageClassName: slow

---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mysql
  storageClassName: slow
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
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
        
        volumeMounts:
        - mountPath: "/var/lib/mysql"
          name: mysql
        ports:
        - containerPort: 3306
      volumes:
      - name: mysql
        persistentVolumeClaim:
          claimName: mysql-pv-claim 

```