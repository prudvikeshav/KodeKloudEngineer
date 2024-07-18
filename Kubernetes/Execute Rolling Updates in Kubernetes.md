**Problem Statement**

#### An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image nginx:1.18 with the latest updates

#### Execute a rolling update for this application, integrating the nginx:1.18 image. The deployment is named nginx-deployment

#### Ensure all pods are operational post-update

**Solution**

#### Check the pods Running

```bash
thor@jumphost ~$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-dnxjs   1/1     Running   0          53s
nginx-deployment-989f57c54-fjvdw   1/1     Running   0          53s
nginx-deployment-989f57c54-zsfs6   1/1     Running   0          53s
```

#### Check the deplopyment

```bash
thor@jumphost ~$ kubectl get deployments.apps 
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           61s
```

#### Verify the deployment to see which image was deployed

```bash
thor@jumphost ~$ kubectl describe deployments.apps nginx-deployment 
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Thu, 18 Jul 2024 07:09:10 +0000
Labels:                 app=nginx-app
                        type=front-end
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx-app
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx-app
  Containers:
   nginx-container:
    Image:         nginx:1.16
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-989f57c54 (3/3 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  86s   deployment-controller  Scaled up replica set nginx-deployment-989f57c54 to 3
```

#### We can see nginx:1.16 was deployed and Strategy was Rolling update

#### Now we need to update the image from nginx:1.16 to nginx:1.18

```bash
kubectl set image deployments/nginx-deployment nginx-container=nginx:1.18
deployment.apps/nginx-deployment image updated
```

#### To check the Rollout was successful or not

```bash
kubectl rollout status deployments/nginx-deployment 
deployment "nginx-deployment" successfully rolled out
```
