### Problem Statement

#### The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want to set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below

#### Create a namespace named dev and deploy a POD within it. Name the pod dev-nginx-pod and use the nginx image with the latest tag. Ensure to specify the tag as nginx:latest

### Solution

#### To create a namespace dev

```bash
kubectl create namespace dev
```

#### To create a pod within the namespace dev

```bash
kubectl run dev-nginx-pod --image=nginx:latest -n dev
```
