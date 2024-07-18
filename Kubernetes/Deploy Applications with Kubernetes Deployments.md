### Problem Statement

#### The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details

#### Create a deployment named nginx to deploy the application nginx using the image nginx:latest (ensure to specify the tag)

##### Note: The kubectl utility on jump_host is set up to interact with the Kubernetes cluster

### Solution

#### TO Create a deployment in Kubernetes

```bash
kubectl create deployment nginx --image=nginx:latest
```

#### To check wheather the deployment has been created

```bash
kubectl get deployments.apps 
> NAME    READY   UP-TO-DATE   AVAILABLE   AGE
> nginx   1/1     1            1           10s
```
