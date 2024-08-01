**Problem Ststement**
One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.



- The deployment name is python-deployment-nautilus, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.

- nodePort should be 32345 and targetPort should be python flask app's default port.

# **Solution:**
```
kubectl describe deployments.apps python-deployment-datacenter 
```
```
Name:                   python-deployment-datacenter
Namespace:              default
CreationTimestamp:      Thu, 01 Aug 2024 02:31:13 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=python_app
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=python_app
  Containers:
   python-container-datacenter:
    Image:         poroko/flask-app-demo
    Port:          5000/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   python-deployment-datacenter-6fdb496d59 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  40s   deployment-controller  Scaled up replica set python-deployment-datacenter-6fdb496d59 to 1
```
After watching the deployment we can see the image name specified wrong. we need to updated it.
```
kubectl edit deployments.apps python-deployment-datacenter 
deployment.apps/python-deployment-datacenter edited
```
```
kubectl get svc
Output
```
NAME                        TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes                  ClusterIP   10.96.0.1      <none>        443/TCP          123m
python-service-datacenter   NodePort    10.96.83.237   <none>        8080:32345/TCP   2m9s
```
Now we can see that container port was 5000 and nodeport port was 8080. we need to updated it also.
```
kubectl edit svc python-service-datacenter 
service/python-service-datacenter edited
```
Click on the app to check its working or not