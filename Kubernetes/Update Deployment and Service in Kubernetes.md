## Problem Statement

 An application deployed on the Kubernetes cluster requires an update with new features developed by the Nautilus application development team. The existing setup includes a deployment named nginx-deployment and a service named nginx-service. Below are the necessary changes to be implemented without deleting the deployment and service.

- Modify the service nodeport from *30008* to *32165*

- Change the replicas count from *1* to *5*

- Update the image from *nginx:1.19* to *nginx:latest*

## Solution

 Check the service running on the cluster.

```bash
kubectl get service
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP        63m
nginx-service   NodePort    10.96.54.165   <none>        80:30008/TCP   100s
```

 We need to update the nodeport from 30008 to 32165.

```bash
kubectl edit service nginx-service
service/nginx-service edited
```

 To check details of the service are correctly edited we use the describe command.

```bash
kubectl describe service nginx-service
Name:                     nginx-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=nginx-app
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.96.54.165
IPs:                      10.96.54.165
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32165/TCP
Endpoints:                10.244.0.5:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

 To check the deployments running on the cluster.

```bash
kubectl get deployments.apps
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/1     1            1           4m37s
```

 We need to update the image and replicas of the deployment nginx-deployment.

```bash
kubectl edit deployments.apps nginx-deployment
deployment.apps/nginx-deployment edited
```

 After updating the replicas has been increased from 1 to 5.

```
kubectl get deployments.apps
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   5/5     5            5           7m6s
```

 To check it is working fine Click on the *app* button on the right top of your browser.
