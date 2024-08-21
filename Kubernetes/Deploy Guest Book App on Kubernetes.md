## Problem Statement

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.

BACK-END TIER

- Create a deployment named *redis-master* for Redis master.

  - Replicas count should be *1*.

  - Container name should be *master-redis-xfusio*n and it should use image *redis*.

  - Request resources as CPU should be *100m* and Memory should be *100Mi*.

  - Container port should be redis default port i.e *6379*.

- Create a service named *redis-master* for Redis master. Port and targetPort should be Redis default port i.e *6379*.

- Create another deployment named *redis-slave* for Redis slave.

  - Replicas count should be *2*.

  - Container name should be *slave-redis-xfusion* and it should use *gcr.io/google_samples/gb-redisslave:v3 image*.

  - Requests resources as CPU should be *100m* and Memory should be *100Mi*.

  - Define an environment variable named *GET_HOSTS_FROM* and its value should be *dns*.

  - Container port should be Redis default port i.e *6379*.

- Create another service named *redis-slave*. It should use Redis default port i.e *6379*.

FRONT END TIER

- Create a deployment named *frontend*.

  - Replicas count should be *3*.

  - Container name should be php-redis-xfusion and it should use *gcr.io/google-samples/gb-frontend@sha256:cbc8ef4b0a2d0b95965e0e7dc8938c270ea98e34ec9d60ea64b2d5f2df2dfbbf image*.

  - Request resources as CPU should be *100m* and Memory should be *100Mi*.

  - Define an environment variable named as *GET_HOSTS_FROM* and its value should be *dns*.

  - Container port should be *80*.

- Create a service named *frontend*. Its type should be NodePort, port should be *80* and its nodePort should be *30009*.

Finally, you can check the guestbook app by clicking on *App* button.

## Solution

### 1. Backend Tier Setup

To deploy the Redis master, use the following manifest file `redis-master.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  strategy: {}
  template:
    metadata:
      
      labels:
        app: redis
    spec:
      containers:
      - image: redis
        name: master-redis-xfusion
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
status: {}

---

apiVersion: v1
kind: Service
metadata:
  
  labels:
    app: redis
  name: redis-master
spec:
  ports:
  - name: redis-master
    port: 6379
    protocol: TCP
    targetPort: 6379
  selector:
    app: redis
  type: ClusterIP
status:
  loadBalancer: {}

---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis-slave
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-slave
  strategy: {}
  template:
    metadata:
      
      labels:
        app: redis-slave
    spec:
      containers:
      - image: gcr.io/google_samples/gb-redisslave:v3
        name: slave-redis-xfusion
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
status: {}

---

apiVersion: v1
kind: Service
metadata:
  
  labels:
    app: redis-slave
  name: redis-slave
spec:
  ports:
  - name: redis-slave
    port: 6379
    protocol: TCP
    targetPort: 6379
  selector:
    app: redis-slave
  type: ClusterIP
status:
  loadBalancer: {}
```

Now we will apply this `backend.yaml` file

```bash
kubectl apply -f backend.yaml 
```

Expected Output:

```
deployment.apps/redis-master created
service/redis-master created
deployment.apps/redis-slave created
service/redis-slave created
```

### 2. Frontend Tier Setup

Create the frontend deployment using the following manifest file `frontend.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  
  labels:
    app: frontend
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  strategy: {}
  template:
    metadata:
      
      labels:
        app: frontend
    spec:
      containers:
      - image: gcr.io/google-samples/gb-frontend@sha256:cbc8ef4b0a2d0b95965e0e7dc8938c270ea98e34ec9d60ea64b2d5f2df2dfbbf
        name: php-redis-xfusion
        ports:
        - containerPort: 80
        resources: 
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
status: {}

---

apiVersion: v1
kind: Service
metadata:
  
  labels:
    app: frontend
  name: frontend
spec:
  ports:
  - name: frontend
    nodePort: 30009
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: frontend
  type: NodePort
status:
  loadBalancer: {}
```

Now we will apply the `frontend.yaml` file.

```bash
kubectl apply -f frontend.yaml 
```

Expected output:

```
deployment.apps/frontend created
service/frontend created
```

### 3. Verify the Deployments, Services, and Pods

To view all the created deployments, services, and pods, use:

```bash
kubectl get all
```

Output:

```
NAME                                READY   STATUS    RESTARTS   AGE
pod/frontend-75d9985df9-685dp       1/1     Running   0          19s
pod/frontend-75d9985df9-hsmhf       1/1     Running   0          19s
pod/frontend-75d9985df9-vdnvw       1/1     Running   0          19s
pod/redis-master-54dd49b469-5tnns   1/1     Running   0          7m58s
pod/redis-slave-67f788df9c-rk4pt    1/1     Running   0          10m
pod/redis-slave-67f788df9c-w4q6w    1/1     Running   0          10m

NAME                   TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/frontend       NodePort    10.96.190.91   <none>        80:30009/TCP   19s
service/kubernetes     ClusterIP   10.96.0.1      <none>        443/TCP        34m
service/redis-master   ClusterIP   10.96.12.9     <none>        6379/TCP       10m
service/redis-slave    ClusterIP   10.96.203.92   <none>        6379/TCP       10m

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/frontend       3/3     3            3           19s
deployment.apps/redis-master   1/1     1            1           10m
deployment.apps/redis-slave    2/2     2            2           10m

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/frontend-75d9985df9       3         3         3       19s
replicaset.apps/redis-master-54dd49b469   1         1         1       7m58s
replicaset.apps/redis-master-6f846d5cf7   0         0         0       10m
replicaset.apps/redis-slave-67f788df9c    2         2         2       10m
```

### 4. Access the Guestbook Application

To access the guestbook application:

Obtain Node IP: Get the IP address of any Kubernetes node. This can be done through your cloud provider's dashboard or by running:

```bash
kubectl get nodes -o wide
```

Access the App: Open a web browser and go to http://<node-ip>:30009, where <node-ip> is the IP address of your Kubernetes node.

Check Application: Verify that the guestbook application is running and accessible.

### Summary

This solution outlines the steps to deploy a guestbook application with Redis master and slave tiers on Kubernetes, along with a frontend tier. It includes creating deployments and services for each component, verifying their status, and accessing the application via a NodePort service.
