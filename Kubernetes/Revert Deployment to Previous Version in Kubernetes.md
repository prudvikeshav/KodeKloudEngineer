## Problem Statement

 Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version

 There exists a deployment named *nginx-deployment*; initiate a *rollback* to the previous revision

## Solution:

 To View the deployments on the cluster.

```bash
kubectl get deployments.apps
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           53s
```

 To view all the deployments with enlarged output.
```bash
kubectl get deployments.apps -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS        IMAGES         SELECTOR
nginx-deployment   3/3     3            3           6m48s   nginx-container   nginx:alpine   app=nginx-app
```

 We can see that the image  was nginx:alpine and we need rollback.

 To rollback the deployment.

```bash
kubectl rollout undo deployment nginx-deployment 
deployment.apps/nginx-deployment rolled back
```

 Now we can see that image was rolled back to nginx:1.16.

```bash
kubectl get deployments.apps -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS        IMAGES       SELECTOR
nginx-deployment   3/3     2            3           7m45s   nginx-container   nginx:1.16   app=nginx-app
```
