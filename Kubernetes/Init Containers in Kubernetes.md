# **Problem Statement**
There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.



Create a Deployment named as ic-deploy-xfusion.


- Configure spec as replicas should be 1, labels app should be ic-xfusion, template's metadata lables app should be the same ic-xfusion.


- The initContainers should be named as ic-msg-xfusion, use image fedora, preferably with latest tag and use command '/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/news'. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.


- Main container should be named as ic-main-xfusion, use image fedora, preferably with latest tag and use command '/bin/bash', '-c' and 'while true; do cat /ic/news; sleep 5; done'. The volume mount should be named as ic-volume-xfusion and mount path should be /ic.


- Volume to be named as ic-volume-xfusion and it should be an emptyDir type.

# **Solution:**

The deplyment file with the requirements mentioned below

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: ic-xfusion
  name: ic-deploy-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  strategy: {}
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      containers:
      - image: fedora:latest
        name: ic-main-xfusion
        command: ["/bin/bash"]
        args: ["-c","while true; do cat /ic/news; sleep 5; done"]
        volumeMounts:
        -  name: ic-volume-xfusion
           mountPath: /ic 

      initContainers: 
      - name: ic-msg-xfusion
        image: fedora:latest
        command: ["/bin/bash"]
        args: ["-c","echo Init Done - Welcome to xFusionCorp Industries > /ic/news"]
        volumeMounts: 
        - name: ic-volume-xfusion
          mountPath: /ic
    volumes:
    - name: ic-volume-xfusion
      emptyDir: {}
    
        resources: {}
status: {}
```