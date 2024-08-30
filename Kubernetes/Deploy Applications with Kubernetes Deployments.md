# Problem Statement

 The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details

 Create a deployment named nginx to deploy the application nginx using the image *nginx:latest* (ensure to specify the tag)

 Note: The kubectl utility on jump_host is set up to interact with the Kubernetes cluster

## Solution

### 1. Create the Deployment

To create a deployment in Kubernetes for the nginx application, use the following command:

```bash
kubectl create deployment nginx --image=nginx:latest
```

### 2. Verify the Deployment

After creating the deployment, you can check if it has been successfully created and is running:

```bash
kubectl get deployments.apps 
```

Expected Output:

```plain
 NAME    READY   UP-TO-DATE   AVAILABLE   AGE
 nginx   1/1     1            1           10s
```
