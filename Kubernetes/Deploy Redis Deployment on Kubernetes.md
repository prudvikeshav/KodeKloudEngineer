# **Problem Statement:**

The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:


- Create a redis deployment with following parameters:

- Create a config map called my-redis-config having maxmemory 2mb in redis-config.

- Name of the deployment should be redis-deployment, it should use
redis:alpine image and container name should be redis-container. Also make sure it has only 1 replica.

- The container should request for 1 CPU.

- Mount 2 volumes:

     -  An Empty directory volume called data at path /redis-master-data.

     - A configmap volume called redis-config at path /redis-master.

     - The container should expose the port 6379.

Finally, redis-deployment should be in an up and running state.


# **Solution:**
First we need to create an configmap 

```
kubectl create configmap my-redis-config --from-literal=maxmemory=2mb
configmap/my-redis-config created
```
Now we need to create an deployment with given requirements

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis-deployment
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-deployment
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: redis-deployment
    spec:
      containers:
      - image: redis:alpine
        name: redis-container
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: 1
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
      volumes:
      - name: redis-config
        configMap: 
          name: my-redis-config
      - name: data
        emptyDir: {}
status: {}

Apply the redis.yaml file 
```
kubectl apply -f redis.yaml 
deployment.apps/redis-deployment created
```