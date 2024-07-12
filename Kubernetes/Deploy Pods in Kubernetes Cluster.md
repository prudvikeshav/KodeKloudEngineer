## Problem Statement

### *The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:*

### *Create a pod named **pod-httpd** using the **httpd** image with the latest tag. Ensure to specify the tag as **httpd:latest**.*

### *Set the app label to **httpd_app**, and name the container as **httpd-container**.*

### *Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.*

## Solution

### Below is the yaml file for creating pod-httpd

```yaml
### Pod-httpd.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: httpd_app
  name: pod-httpd
spec:
  containers:
  - image: httpd:latest
    name: httpd_container
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

### Run the pod yaml file

```bash
thor@jumphost ~$ kubectl apply -f pod.yaml 
pod/pod-httpd created
```
