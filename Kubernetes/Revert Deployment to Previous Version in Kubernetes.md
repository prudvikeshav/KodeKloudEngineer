## Problem Statement

Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

There exists a deployment named *nginx-deployment*; initiate a *rollback* to the previous revision.

## Solution

To rollback the *nginx-deployment* to its previous revision, follow these steps:

### 1. View the Deployments on the Cluster

First, verify the existing deployments to confirm the status and details of *nginx-deployment*.

```bash
kubectl get deployments.apps
```

Output:

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           53s
```

### 2. View Detailed Information about the Deployment

Get detailed information about the deployment, including the container image versions.

```bash
kubectl get deployments.apps -o wide
```

Output:

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS        IMAGES         SELECTOR
nginx-deployment   3/3     3            3           6m48s   nginx-container   nginx:alpine   app=nginx-app
```

This confirms that the current image version is `nginx:alpine`.

### 3. Rollback the Deployment

To rollback the deployment to the previous revision, use the following command:

```bash
kubectl rollout undo deployment nginx-deployment
```

Output:

```
deployment.apps/nginx-deployment rolled back
```

### 4. Verify the Rollback

After the rollback, verify that the deployment has reverted to the previous image version.

```bash
kubectl get deployments.apps -o wide
```

Output:

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS        IMAGES       SELECTOR
nginx-deployment   3/3     3            3           7m45s   nginx-container   nginx:1.16   app=nginx-app
```

This confirms that the deployment has been successfully rolled back to the previous image version `nginx:1.16`.

By following these steps, you have successfully rolled back the *nginx-deployment* to the previous revision, resolving the reported issue.
