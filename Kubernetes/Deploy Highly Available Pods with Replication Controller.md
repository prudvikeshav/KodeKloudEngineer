## Problem Statement

 The Nautilus DevOps team is establishing a ReplicationController to deploy multiple pods for hosting applications that require a highly available infrastructure. Follow the specifications below to create the ReplicationController

 Create a *ReplicationController* using the *nginx* image, preferably with latest tag, and name it *nginx-replicationcontroller*

 Assign labels *app* as *nginx_app*, and *type* as *front-end*. Ensure the container is named *nginx-container* and set the replica count to *3*.

 All pods should be running state post-deployment

## Solution

### 1. Creating the ReplicationController

To deploy a highly available application using a ReplicationController, you need to create a YAML manifest that specifies the details of the ReplicationController. Below is the YAML configuration for the `nginx-replicationcontroller`:

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

 Create the ReplicationController:

Save the above YAML configuration into a file named `replication.yaml`, then apply it with:

```bash
kubectl apply -f replication.yaml 
```

Expected Output:

```
replicationcontroller/nginx-replicationcontroller created
```

### 2. Verifying the ReplicationController

To ensure that the ReplicationController has been created successfully and is managing the desired number of pods, you can use the following commands:

Check the ReplicationController:

```bash
kubectl get replicationcontrollers 
```

Expected Output:

```
NAME                          DESIRED   CURRENT   READY   AGE
nginx-replicationcontroller   3         3         3       37s
```

This output indicates that the ReplicationController is managing 3 replicas, all of which are currently running.

Describe the ReplicationController for detailed information:

```bash
kubectl describe replicationcontrollers nginx-replicationcontroller 
```

Expected Output:

```yaml
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

### Summary

This solution demonstrates how to create and verify a ReplicationController in Kubernetes to manage a set of nginx pods. The detailed steps include creating a YAML manifest, applying it to create the ReplicationController, and verifying that it is functioning as expected by checking the status and details of the ReplicationController and its managed pods.
