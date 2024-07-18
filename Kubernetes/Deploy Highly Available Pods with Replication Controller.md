**Problem Statement**

#### The Nautilus DevOps team is establishing a ReplicationController to deploy multiple pods for hosting applications that require a highly available infrastructure. Follow the specifications below to create the ReplicationController

#### Create a ReplicationController using the nginx image, preferably with latest tag, and name it nginx-replicationcontroller

#### Assign labels app as nginx_app, and type as front-end. Ensure the container is named nginx-container and set the replica count to 3

#### All pods should be running state post-deployment

**Solution**

#### Create a replication controller using the given details

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-replicationcontroller
spec:
  replicas: 3
  selector:
    app: nginx_app
    type: front-end
  template:
    metadata:
      name: nginx-replicationcontroller
      labels:
        app: nginx_app
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

#### Apply the created replication yaml file

```bash
kubectl apply -f replication.yaml 
```

```
replicationcontroller/nginx-replicationcontroller created
```

#### Check the replication controller created

```bash
kubectl get replicationcontrollers 
```

```
NAME                          DESIRED   CURRENT   READY   AGE
nginx-replicationcontroller   3         3         3       37s
```

#### Verify the details applied corectly apllied or not

```bash
kubectl describe replicationcontrollers nginx-replicationcontroller 
```

```
Name:         nginx-replicationcontroller
Namespace:    default
Selector:     app=nginx_app,type=front-end
Labels:       app=nginx_app
              type=front-end
Annotations:  <none>
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx_app
           type=front-end
  Containers:
   nginx-container:
    Image:         nginx:latest
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age   From                    Message
  ----    ------            ----  ----                    -------
  Normal  SuccessfulCreate  51s   replication-controller  Created pod: nginx-replicationcontroller-ccmzz
  Normal  SuccessfulCreate  51s   replication-controller  Created pod: nginx-replicationcontroller-5xnrp
  Normal  SuccessfulCreate  51s   replication-controller  Created pod: nginx-replicationcontroller-l6z6p
```
