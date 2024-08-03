## Problem Statement

 The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements

-  We already have a secret key file *official.txt* under */opt* location on jump host. Create a generic secret named *official*, it should contain the password/license-number present in *official.txt* file

-  Also create a pod named *secret-devops*

-  Configure pod's spec as container name should be *secret-container-devops*, image should be centos preferably with latest tag (remember to mention the tag with image). Use *sleep* command for container so that it remains in running state. Consume the created secret and mount it under */opt/demo* within the container

-  To verify you can exec into the container *secret-container-devops*, to check the secret key under the mounted path */opt/demo*. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience

## Solution:

 Create a secret with the file.

```bash
kubectl create secret generic official --from-file=password=/opt/official.txt
```

```
secret/official created
```

 Now we need to create an pod with image *centos:latest* and secret should be mount on */opt/demo* in container.

```yaml
apiVersion: v1
kind: Pod
metadata:
   name: secret-devops
spec:
   containers:
   -  name: secret-container-devops
      image: centos:latest
      command:
        - 'bin/bash'
        -  '-c'
        -  sleep
      volumeMounts:
      -  name: secret-volume
         mountPath: /opt/demo
         readOnly: true
   volumes:
   - name: secret-volume
     secret:
       secretName: official
```

 Apply to the pod file

```bash
kubectl apply -f pod.yaml
```

```
pod/secret-devops created
```

 We need to check if the secret mounted on the container in */opt/demo* location.

```bash
kubectl exec -it secret-devops sh
```

```
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
sh-4.4# cat /opt/demo/password
5ecur3
sh-4.4#
```
