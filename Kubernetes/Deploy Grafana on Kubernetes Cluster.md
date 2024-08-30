# Problem Statement

 The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details

- Create a deployment named *grafana-deployment-devops* using any *grafana* image for Grafana app. Set other parameters as per your choice

- Create *NodePort* type service with nodePort *32000* to expose the app

## Solution

### 1. Create the Deployment

To deploy Grafana on Kubernetes, use the following command:

```bash
kubectl create deployment grafana-deployment-devops --image=grafana/grafana
```

### 2. Verify the Deployment

To check if the Grafana pod is running, use:

```bash
kubectl get pods
```

Expected Output:

```plain
NAME                                       READY   STATUS    RESTARTS   AGE
grafana-deployment-devops-9b97dff6-kjxf2   1/1     Running   0          25s
```

### 3. Create the Service

To expose the Grafana application using a NodePort service, execute:

```bash
kubectl create svc nodeport grafana-service-devops --tcp=80:3000
```

Expected Output:

```plain
service/grafana-service-devops created
```

### 4. Verify the Service

To check the status and configuration of the service, use:

```bash
kubectl get svc
```

Expected Output:

```plain
NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
grafana-service-devops   NodePort    10.96.158.245   <none>        80:32000/TCP   77s
kubernetes               ClusterIP   10.96.0.1       <none>        443/TCP        72m
```

### 5. Detailed Service Information

To get detailed information about the service configuration, including endpoints and port mappings, use:

```bash
kubectl describe svc grafana-service-devops
```

Expected Output:

```plain
Name:                     grafana-service-devops
Namespace:                default
Labels:                   app=grafana-service-devops
Annotations:              <none>
Selector:                 app=grafana-deployment-devops
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.96.158.245
IPs:                      10.96.158.245
Port:                     grafana  80/TCP
TargetPort:               3000/TCP
NodePort:                 grafana  32000/TCP
Endpoints:                10.244.0.6:3000
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

### Summary

This guide demonstrates how to deploy Grafana on a Kubernetes cluster, expose it via a NodePort service, and access it from outside the cluster. The steps include creating a Grafana deployment, exposing it through a NodePort service, and verifying the setup using Kubernetes commands. This setup will allow you to collect and analyze metrics from your applications using Grafana's powerful visualization tools.
