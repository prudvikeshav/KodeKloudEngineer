## Problem Statement

 There is an iron gallery run that the Nautilus DevOps team was developing. They have recently customized the run and are going to deploy the same on the Kubernetes cluster. Below you can find more details

- Create a namespace *iron-namespace-datacenter*

- Create a deployment *iron-gallery-deployment-datacenter* for iron gallery under the same namespace you created

  - Labels *run* should be *iron-gallery*

  - Replicas count should be *1*

  - Selector's matchLabels *run* should be *iron-gallery*

  - Template labels *run* should be *iron-gallery* under metadata

  - The container should be named as *iron-gallery-container-datacenter*, use *kodekloud/irongallery:2.0* image ( use exact image name / tag )

  - Resources limits for memory should be *100Mi* and for CPU should be *50m*

  - First volumeMount name should be *config*, its mountPath should be */usr/share/nginx/html/data*

  - Second volumeMount name should be *images*, its mountPath should be */usr/share/nginx/html/uploads*

  - First volume name should be config and give it *emptyDir* and second volume name should be images, also give it *emptyDir*

- Create a deployment *iron-db-deployment-datacenter* for iron db under the same namespace

  - Labels db should be *mariadb*

  - Replicas count should be *1*

  - Selector's matchLabels db should be *mariadb*

  - Template labels *db* should be *mariadb* under metadata

  - The container name should be *iron-db-container-datacenter*, use *kodekloud/irondb:2.0* image ( use exact image name / tag )

  - Define environment, set *MYSQL_DATABASE* its value should be *database_host*, set *MYSQL_ROOT_PASSWORD* and *MYSQL_PASSWORD* value should be with some complex              passwords for DB connections, and *MYSQL_USER* value should be any custom user ( except root )

  - Volume mount name should be db and its mountPath should be */var/lib/mysql*. Volume name should be db and give it an *emptyDir*.

- Create a service for iron db which should be named *iron-db-service-datacenter* under the same namespace. Configure spec as selector's db should be *mariadb*. Protocol should be TCP, port and targetPort should be *3306* and its type should be ClusterIP

- Create a service for *iron gallery* which should be named *iron-gallery-service-datacenter* under the same namespace. Configure spec as selector's *run* should be *iron-gallery*. Protocol should be TCP, port and targetPort should be *80*, nodePort should be *32678* and its type should be *NodePort*

## Solution

### 1. Create the Namespace

Create the namespace `iron-namespace-datacenter` to isolate the deployments and services related to the Iron Gallery application:

```bash
kubectl create namespace iron-namespace-xfusion
```

Expected Output:

```arduino
namespace/iron-namespace-xfusion created
```

### 2. Deploy Iron Gallery and Iron DB

Create a YAML manifest file `iron-gallery.yaml` for the Iron Gallery deployment, Iron DB deployment, and their corresponding services:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: iron-gallery
  name: iron-gallery-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  strategy: {}
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
      - image: kodekloud/irongallery:2.0
        name: iron-gallery-container-xfusion
        resources: 
          requests:
            cpu: "50m"
            memory: "100Mi"
          limits:
            cpu: "50m"
            memory: "100Mi"
        volumeMounts:
        - name: config
          mountPath: /usr/share/nginx/html/data
        - name: images
          mountPath: /usr/share/nginx/html/uploads
      volumes: 
      - name: config
        emptyDir: {}
      - name: images
        emptyDir: {}
---      

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    db: mariadb
  name: iron-db-deployment-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  strategy: {}
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
      - image: kodekloud/irondb:2.0
        name: iron-db-container-xfusion
        env:
        - name: MYSQL_DATABASE
          value: database_host
        - name: MYSQL_ROOT_PASSWORD
          value: data_password
        - name: MYSQL_PASSWORD
          value: db_password
        - name: MYSQL_USER
          value: db_admin
        volumeMounts:
        - name: db
          mountPath: /var/lib/mysql
       
      volumes: 
      - name: db
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    db: mariadb
  name: iron-db-service-xfusion
spec:
  ports:
  - name: irondb
    port: 3306
    protocol: TCP
    targetPort: 3306
  selector:
    db: mariadb
  type: ClusterIP
status:
  loadBalancer: {}    

---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    run: iron-gallery
  name: iron-gallery-service-xfusion
spec:
  ports:
  - name: irongallery
    nodePort: 32678
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    run: iron-gallery
  type: NodePort
status:
  loadBalancer: {}
```

Explanation:

- Namespace: All deployments and services are created within the iron-namespace-datacenter namespace.
- Deployments:
  - Iron Gallery: Uses the image kodekloud/irongallery:2.0, with specified resource limits, volume mounts, and volume definitions.
  - Iron DB: Uses the image kodekloud/irondb:2.0, sets up environment variables for database configuration, and uses an emptyDir volume.
- Services:
  - Iron DB Service: Exposes the database on port 3306 with ClusterIP type.
  - Iron Gallery Service: Exposes the gallery application on port 80 with NodePort type and nodePort 32678.

Apply the iron gallery manifest file:

```
kubectl apply -f irongallery.yaml
```

Expected Output:

```
deployment.apps/iron-gallery-deployment-xfusion created
deployment.apps/iron-db-deployment-xfusion unchanged
service/iron-db-service-xfusion configured
service/iron-gallery-service-xfusion configured
```

3. Verify the Deployments and Services
To ensure that everything is running correctly, you can check the status of deployments and services:

Check deployments:

```bash
kubectl get deployments -n iron-namespace-datacenter
```

Expected Output:

```bash
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
iron-gallery-deployment-datacenter   1/1     1            1           <AGE>
iron-db-deployment-datacenter        1/1     1            1           <AGE>
```

Check services:

```bash
kubectl get services -n iron-namespace-datacenter
```

Expected Output:

```bash
NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
iron-db-service-datacenter       ClusterIP   10.96.0.1       <none>        3306/TCP         <AGE>
iron-gallery-service-datacenter  NodePort    10.96.0.2       <none>        80:32678/TCP     <AGE>
```

This ensures that the deployments and services are correctly set up and the pods are running as expected.
